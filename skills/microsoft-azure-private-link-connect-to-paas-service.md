---
name: connect-to-paas-service
description: >-
  Create a private endpoint onto an Azure PaaS resource so traffic reaches it over a private IP inside
  your virtual network, then attach the private DNS zone group that makes the service's hostname actually
  resolve to it. Covers the whole consumer-side flow including the asynchronous wait everyone skips.
api: microsoft-azure-private-link
api_version: '2025-03-01'
base_url: https://management.azure.com
operations:
  - AvailablePrivateEndpointTypes_List
  - PrivateEndpoints_CreateOrUpdate
  - PrivateEndpoints_Get
  - PrivateDnsZoneGroups_CreateOrUpdate
  - PrivateDnsZoneGroups_Get
generated: '2026-09-17'
method: generated
source: openapi/_original/microsoft-azure-private-link-private-endpoint-swagger.json
---

# Connect a virtual network to an Azure PaaS service privately

Every request needs `?api-version=2025-03-01` and an `Authorization: Bearer` token from Microsoft
Entra ID for the `https://management.azure.com/.default` scope. There is no API key. You also need RBAC
write on the target subnet — Network Contributor covers it.

## 1. Confirm the target supports a private endpoint, and learn its group id

`AvailablePrivateEndpointTypes_List`

```
GET /subscriptions/{subscriptionId}/providers/Microsoft.Network/locations/{location}/availablePrivateEndpointTypes?api-version=2025-03-01
```

Response is `{value: [...], nextLink}` — follow `nextLink` verbatim if present; there is no page-size
parameter. Each entry names a `resourceName` such as `Microsoft.Storage/storageAccounts`. The **group id**
(`blob`, `table`, `sqlServer`, `vault`…) is the sub-resource you are connecting to, and you need it in
step 2. It is documented per-service in the Private Link availability matrix, not returned here.

## 2. Create the private endpoint

`PrivateEndpoints_CreateOrUpdate`

```
PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateEndpoints/{privateEndpointName}?api-version=2025-03-01
```

Body, at minimum:

```json
{
  "location": "eastus",
  "properties": {
    "subnet": { "id": "/subscriptions/.../virtualNetworks/{vnet}/subnets/{subnet}" },
    "privateLinkServiceConnections": [{
      "name": "conn",
      "properties": {
        "privateLinkServiceId": "/subscriptions/.../providers/Microsoft.Storage/storageAccounts/{name}",
        "groupIds": ["blob"]
      }
    }]
  }
}
```

Use `privateLinkServiceConnections` when you own the target or it auto-approves you. Use
`manualPrivateLinkServiceConnections` — with a `requestMessage` — when the target belongs to someone else
and a human has to approve; the connection then sits in `Pending` until they act.

**This is a long-running operation.** A 201 means accepted, not created. Poll the URL in the
`Azure-AsyncOperation` response header until it reports `Succeeded`, `Failed` or `Canceled`, or poll
`PrivateEndpoints_Get` until `properties.provisioningState` is `Succeeded`. Do not proceed on the 201.

Replaying this PUT is safe in the sense that it converges on the same resource — there is no
`Idempotency-Key` and no dedupe window, so a retry while the first call is still provisioning is accepted
again and gives you a second operation to poll. A PUT also replaces the **whole** resource: if you are
modifying an existing endpoint, GET it first, edit the body you got back, and send `If-Match` with its
`etag` so you do not silently revert someone else's change.

## 3. Attach a private DNS zone group

Skip this and the endpoint exists with a private IP while the PaaS hostname keeps resolving to its public
address. Nothing routes privately.

`PrivateDnsZoneGroups_CreateOrUpdate`

```
PUT /subscriptions/{subscriptionId}/resourceGroups/{rg}/providers/Microsoft.Network/privateEndpoints/{privateEndpointName}/privateDnsZoneGroups/{privateDnsZoneGroupName}?api-version=2025-03-01
```

```json
{
  "properties": {
    "privateDnsZoneConfigs": [{
      "name": "config",
      "properties": { "privateDnsZoneId": "/subscriptions/.../privateDnsZones/privatelink.blob.core.windows.net" }
    }]
  }
}
```

Also long-running. Afterwards `PrivateDnsZoneGroups_Get` returns the `recordSets` Azure wrote on your
behalf — read-only, and the proof the binding took.

If you run your own DNS instead, skip the zone group and read
`PrivateEndpoints_Get` → `properties.customDnsConfigs`, which gives you the `fqdn` and `ipAddresses` pairs
to create yourself.

## 4. Verify

`PrivateEndpoints_Get` and check:

- `properties.provisioningState` is `Succeeded`
- `properties.privateLinkServiceConnections[].properties.privateLinkServiceConnectionState.status` is
  `Approved` (a manual connection will read `Pending` — the other party has not acted yet)
- `properties.networkInterfaces[]` names the NIC Azure created, whose IP config holds the private IP

## Failure handling

Errors come back as `{"error": {"code", "message", "target", "details", "innerError"}}` — Azure's own
envelope, not RFC 9457, and the contract types none of them. Branch on `error.code`:

| Code | Status | What to do |
|---|---|---|
| `MissingApiVersionParameter` | 400 | Add `?api-version=` — it is required on every call |
| `AuthenticationFailed` | 401 | Token missing or expired; re-acquire for `https://management.azure.com/.default` |
| `AuthorizationFailed` | 403 | RBAC — you need write on the subnet and on the target resource |
| `RequestDisallowedByPolicy` | 403 | An Azure Policy assignment blocked it; the body names the policy |
| `SubscriptionNotFound` / `ResourceGroupNotFound` / `ResourceNotFound` | 404 | Check the ids in the path |
| `MissingSubscriptionRegistration` | 409 | `az provider register --namespace Microsoft.Network`, once per subscription |
| `ThrottledRequest` | 429 | Honour `Retry-After` (seconds). Writes draw on a 200-token bucket refilling at 10/s, and 1,000 per 5 minutes at the Microsoft.Network provider |

A long-running operation that fails does **not** surface the error on the original call. It appears as
`provisioningState: Failed` with the same envelope on the polled operation. Code that only checks the
first response will report success for a create that never completed.

## Undoing this

`PrivateEndpoints_Delete` is asynchronous and **irreversible** — no soft delete, no restore, no published
retention window. The private IP returns to the subnet and the DNS records go with it. Recreating gives
you a new resource whose connection the service owner must approve again. GET the endpoint and keep the
body before deleting it; that capture is your only rollback.
