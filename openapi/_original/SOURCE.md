# Verbatim provider contracts — Azure Private Link

Both files were fetched unmodified on 2026-09-17 from Microsoft's own specification repository,
`Azure/azure-rest-api-specs`, `api-version` **2025-03-01** (the last stable version in which Private Link
is published as its own document; from `2025-05-01` onward Microsoft folded the same paths into
`virtualNetwork.json`).

| Saved as | Upstream |
|---|---|
| `microsoft-azure-private-link-private-endpoint-swagger.json` | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/specification/network/resource-manager/Microsoft.Network/Network/stable/2025-03-01/privateEndpoint.json |
| `microsoft-azure-private-link-private-link-service-swagger.json` | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/specification/network/resource-manager/Microsoft.Network/Network/stable/2025-03-01/privateLinkService.json |

**Ownership check (STEP 0c).** Both are Swagger 2.0, `info.title` *NetworkManagementClient*,
`host: management.azure.com`, `securityDefinitions.azure_auth` pointing at
`login.microsoftonline.com`, served from the `Azure` GitHub organization. Host, title, auth server and
publishing org all name Microsoft. Nothing in either document points at another company.

**Unresolved external `$ref`s.** Microsoft splits the Microsoft.Network surface across sibling documents,
so these two carry relative `$ref`s to `./network.json`, `./networkInterface.json`,
`./virtualNetwork.json`, `./loadBalancer.json`, `./applicationSecurityGroup.json` and each other, plus
`./examples/*.json`. Those siblings are NOT copied here — they describe other Azure resource types, not
Private Link. To resolve the documents fully, fetch them from the same upstream directory. Renaming the
files to the pipeline's slug-prefixed convention is what breaks the relative paths; the documents
themselves are byte-identical to what Microsoft publishes.
