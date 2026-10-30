# Microsoft Azure Private Link

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Microsoft Azure Private Link gives a virtual network a private IP address onto an Azure PaaS service, a partner service, or a service the customer publishes themselves, so traffic reaches it across the Microsoft backbone and never traverses the public internet. It has two sides. A consumer creates a private endpoint pointing at a target resource id and attaches a private DNS zone group so the service's own hostname resolves to that private address. A producer publishes a private link service in front of a Standard Load Balancer, hands out an opaque alias, and approves or rejects the connections that arrive — including connections from other tenants. Both sides are managed through the Azure Resource Manager REST API at management.azure.com, which Microsoft publishes as Swagger 2.0 in its azure-rest-api-specs repository: 24 operations at api-version 2025-03-01, OAuth2 bearer only, with no API key and no anonymous surface.

**Areas:** Networking · Private Connectivity · Cloud Infrastructure · Virtual Networks · DNS · Security · Azure Resource Manager · Infrastructure-as-a-Service

## Microsoft Azure Private Link on APIs.io

Everything profiled for Microsoft Azure Private Link is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/microsoft-azure-private-link/](https://apis.io/providers/microsoft-azure-private-link/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [microsoft-azure-private-link Operations API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-operations-api/) — Operations operations
- [microsoft-azure-private-link Private Endpoints API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-private-endpoints-api/) — Private Endpoints operations
- [Microsoft Azure Private Link Private Dns Zone Groups API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-privatednszonegroups-api/) — The PrivateDnsZoneGroups API from Microsoft Azure Private Link — 2 operation(s) for privatednszonegroups.
- [Microsoft Azure Private Link Private Link Service API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-privatelinkservice-api/) — The PrivateLinkService API from Microsoft Azure Private Link — 1 operation(s) for privatelinkservice.
- [Microsoft Azure Private Link Private Link Services API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-privatelinkservices-api/) — The PrivateLinkServices API from Microsoft Azure Private Link — 9 operation(s) for privatelinkservices.
- [Microsoft Azure Private Link Subscriptions API](https://apis.io/apis/microsoft-azure-private-link/microsoft-azure-private-link-subscriptions-api/) — The Subscriptions API from Microsoft Azure Private Link — 2 operation(s) for subscriptions.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [microsoft-azure-private-link-mcp.yml](https://apis.io/servers/microsoft-azure-private-link/microsoft-azure-private-link-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Microsoft Azure Private Link Agentic Access](https://apis.io/agentic-access/microsoft-azure-private-link/microsoft-azure-private-link-agentic-access/)

#### Collections (3)

Postman and Open Collection request collections.

- [Azure Private Link REST API](https://apis.io/collections/microsoft-azure-private-link/open-microsoft-azure-private-link/)
- [Azure Private Link REST Operations API](https://apis.io/collections/microsoft-azure-private-link/open-microsoft-azure-private-link-operations-api/)
- [Azure Private Link REST Operations Private Endpoints API](https://apis.io/collections/microsoft-azure-private-link/open-microsoft-azure-private-link-private-endpoints-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Microsoft Azure Private Link Operations Api Overlay](https://apis.io/overlays/microsoft-azure-private-link/microsoft-azure-private-link-operations-api-overlay/)
- [Microsoft Azure Private Link Private Endpoints Api Overlay](https://apis.io/overlays/microsoft-azure-private-link/microsoft-azure-private-link-private-endpoints-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Microsoft Azure Private Link Authentication](https://apis.io/security/microsoft-azure-private-link/microsoft-azure-private-link-authentication/)
- [Microsoft Azure Private Link Domain Security](https://apis.io/security/microsoft-azure-private-link/microsoft-azure-private-link-domain-security/)
- [Microsoft Azure Private Link Trust Center](https://apis.io/security/microsoft-azure-private-link/microsoft-azure-private-link-trust-center/)
- [Microsoft Azure Private Link Vulnerability Disclosure](https://apis.io/security/microsoft-azure-private-link/microsoft-azure-private-link-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Microsoft Azure Private Link Scopes](https://apis.io/scopes/microsoft-azure-private-link/microsoft-azure-private-link-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Microsoft Azure Private Link Plans Pricing](https://apis.io/plans/microsoft-azure-private-link/microsoft-azure-private-link-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Microsoft Azure Private Link Rate Limits](https://apis.io/rate-limits/microsoft-azure-private-link/microsoft-azure-private-link-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Microsoft Azure Private Link Finops](https://apis.io/finops/microsoft-azure-private-link/microsoft-azure-private-link-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.microsoft.com/)
- [Documentation](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview)
- [Getting started](https://learn.microsoft.com/en-us/azure/private-link/create-private-endpoint-portal)
- [API reference](https://learn.microsoft.com/en-us/rest/api/virtualnetwork/private-endpoints)
- [Pricing](https://azure.microsoft.com/en-us/pricing/details/private-link/)
- [Sign up](https://azure.microsoft.com/en-us/free/)
- [Support](https://azure.microsoft.com/en-us/support/options/)
- [Blog](https://techcommunity.microsoft.com/category/azure/blog/azurenetworkingblog)
- [Security](https://www.microsoft.com/en-us/msrc/cvd)
- [Compliance](https://learn.microsoft.com/en-us/azure/compliance/)
- [Terms of service](https://www.microsoft.com/en-us/legal/terms-of-use)
- [Privacy policy](https://privacy.microsoft.com/en-us/privacystatement)
- [GitHub](https://github.com/Azure)
- [Status](https://azure.status.microsoft/en-us/status)

## API Evangelist rating

**Kin Score 62.8** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 42.2 |
| Contract governance | 4.5 |
| Operational transparency | 89.5 |
| Developer ergonomics | 73.2 |
| Access clarity | 60.5 |

**Agent readiness 27.7** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | derived (partial credit) |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/microsoft-azure-private-link/](https://apis.io/providers/microsoft-azure-private-link/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Microsoft Azure Private Link: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/microsoft-azure-private-link/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
