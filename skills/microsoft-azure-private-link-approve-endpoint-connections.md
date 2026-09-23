---
name: approve-endpoint-connections
description: >-
  Operate the producer side of Azure Private Link — publish a private link service, find the consumer
  connections waiting on you, and approve or reject them. Includes the asymmetry that makes rejection a
  one-way door.
api: microsoft-azure-private-link
api_version: '2025-03-01'
base_url: https://management.azure.com
operations:
  - PrivateLinkServices_CreateOrUpdate
  - PrivateLinkServices_Get
  - PrivateLinkServices_ListPrivateEndpointConnections
  - PrivateLinkServices_GetPrivateEndpointConnection
  - PrivateLinkServices_UpdatePrivateEndpointConnection
  - PrivateLinkServices_DeletePrivateEndpointConnection
generated: '2026-09-17'
method: generated
source: openapi/_original/microsoft-azure-private-link-private-link-service-swagger.json
---

# Publish a private link service and manage who connects to it

You are the producer here: you run a service behind a Standard Load Balancer and want named consumers to
reach it privately, possibly across tenants. Same auth as everywhere on this API — Entra ID bearer token
for `https://management.azure.com/.default`, `?api-version=2025-03-01` on every call.

## 1. Publish the service

`PrivateLinkServices_CreateOrUpdate`

```
PUT /subscriptions/{subscriptionId}/resourceGroups/{rg}/providers/Microsoft.Network/privateLinkServices/{serviceName}?api-version=2025-03-01
```

The properties that decide the access model:

- `loadBalancerFrontendIpConfigurations[]` — the Standard Load Balancer frontend to publish. Required.
- `ipConfigurations[]` — the NAT IPs in your subnet that consumer traffic will appear to come from.
- `visibility.subscriptions[]` — who is allowed to *see* the alias. `["*"]` means anyone with the alias.
- `autoApproval.subscriptions[]` — whose connection is accepted with no human in the loop. Everyone else
  lands in `Pending` and waits for step 3.
- `enableProxyProtocol` — set this if your backend needs the consumer's real source address.

Long-running: poll `Azure-AsyncOperation` until `Succeeded`.

Then `PrivateLinkServices_Get` and read `properties.alias`. That opaque string is what you hand to
consumers; it lets them connect without being able to enumerate your resources.

## 2. See who is waiting

`PrivateLinkServices_ListPrivateEndpointConnections`

```
GET /subscriptions/{subscriptionId}/resourceGroups/{rg}/providers/Microsoft.Network/privateLinkServices/{serviceName}/privateEndpointConnections?api-version=2025-03-01
```

Paged as `{value, nextLink}`. Each `PrivateEndpointConnection` carries:

- `properties.privateLinkServiceConnectionState.status` — `Pending`, `Approved`, `Rejected` or
  `Disconnected`
- `properties.privateLinkServiceConnectionState.description` — the consumer's `requestMessage`, which is
  the only context you get about who is asking
- `properties.privateEndpoint` — the consumer's endpoint resource id
- `properties.privateEndpointLocation` and `properties.linkIdentifier`

Filter for `status: Pending` to get your work queue.

## 3. Approve or reject

`PrivateLinkServices_UpdatePrivateEndpointConnection`

```
PUT /subscriptions/{subscriptionId}/resourceGroups/{rg}/providers/Microsoft.Network/privateLinkServices/{serviceName}/privateEndpointConnections/{peConnectionName}?api-version=2025-03-01
```

```json
{
  "properties": {
    "privateLinkServiceConnectionState": {
      "status": "Approved",
      "description": "approved for the analytics team",
      "actionsRequired": "None"
    }
  }
}
```

This is the one operation in the whole Private Link API that is **not** long-running — 200 only, no
polling. It is also the one genuinely reversible write.

**The asymmetry, and it matters.** `Approved → Rejected` works and takes effect immediately. `Rejected →
Approved` does **not**: once you reject, the consumer has to delete their private endpoint and create a
new one to try again. Reject only when you mean it; if you are unsure, leave it `Pending`.

`actionsRequired` is free text the consumer reads on their side. Use it to tell them what is missing
rather than rejecting them.

## 4. Disconnect an approved consumer

`PrivateLinkServices_DeletePrivateEndpointConnection` drops the connection from your side. It is
long-running and irreversible — you cannot restore it, and the consumer must create a new endpoint. For a
reversible pause, set `status` to `Rejected` instead; the connection object survives.

## Checking visibility before you tell someone the alias

`PrivateLinkServices_CheckPrivateLinkServiceVisibility` (POST, long-running despite changing nothing)
answers whether a given `privateLinkServiceAlias` is visible to the calling subscription. Use it to
confirm your `visibility.subscriptions[]` list does what you think before a consumer discovers it does not.

`PrivateLinkServices_ListAutoApprovedPrivateLinkServices` lists, per region, the services that would
auto-approve the calling subscription — the consumer-facing counterpart of the same setting.

## Failure handling

Same ARM envelope as the rest of the API: `{"error": {"code", "message", ...}}`, no `problem+json`, no
typed 4xx in the contract. `AuthorizationFailed` on step 3 usually means you are missing the
`Microsoft.Network/privateLinkServices/privateEndpointConnections/write` action rather than anything about
the consumer. On 429, honour `Retry-After`; writes here draw from the same 200-token/10-per-second ARM
bucket as everything else.

## Undoing this

`PrivateLinkServices_Delete` is asynchronous and irreversible, and it fans out — every consumer endpoint
connected to the service is disconnected, and the alias is not reissued if you recreate the service. Every
consumer would have to be re-onboarded. GET the service and store the body first.
