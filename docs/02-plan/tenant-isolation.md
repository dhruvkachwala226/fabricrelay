# Shared-database tenant isolation

The Business ID is the tenancy root. Fabric, Inquiry, tenant-scoped User, tenant AuditLog events and tenant operational records carry businessId explicitly. Catalog, Colorway and attached media inherit ownership from their parent. Platform Users and platform-only operational or audit records have explicit global semantics. The server derives private scope from its verified principal. A wholesaler request schema has no businessId field, and an object ID, colorway ID, asset ID or cursor is never proof of ownership.

| Boundary | Control |
| --- | --- |
| Detail reads and mutations | Filter by owner and record ID in the database operation. Match colorway and media updates to the already scoped parent. Match foreign references and asset initialization to the same tenant. A missing or alien object returns the same 404. |
| Lists and pagination | Set owner and allowed filter keys on the server. Sign or validate opaque cursors against tenant, query and ordering. Bound page size. |
| Role escalation | Schema allowlists reject role, status, owner and audit keys in profile and feature writes. Role and lifecycle updates go through platform workflows. Read authoritative role on each protected request. |
| Super Admin | Use explicit platform routes for business lifecycle, wholesaler provisioning, catalog metadata and audit. Record business-targeted mutations in the same transaction. Platform has no tenant login, draft media or inquiry access by role inheritance. |
| Public surfaces | Resolve canonical slug and require active business and published catalog and listing as appropriate. Use allowlisted projections. Never expose contact, rate fields in quote-only mode, internal IDs or provider asset identifiers. Use the same eligibility rule on page, API and image requests. |
| Sessions and caching | Recheck status before side effects. Read from authoritative state and use no-store for private or visibility-sensitive data. A request already in progress may race a state change. Public intake fences business and listing changes in its transaction. |
| Image operations | Scope signing, confirmation, reference deletion and private delivery to the authorized parent. Revalidate publication for public delivery. Server-owned IDs drive deletion only after reference removal. |

## Verification strategy

Use two active tenants with identical design codes and colorway names, plus suspended business, suspended user and Super Admin fixtures. In each positive test, verify a same-tenant record works. Substitute the second tenant's identifiers in URL, cursor, nested references, API payload and media confirmation. Assert deterministic denials, zero foreign writes and no private fields in responses. Exercise direct REST access and direct page, fabric and media URLs after suspension and unpublishing. Reuse cookies across suspend and reactivate tests to observe revocation. Attempt tenant status and role mutation through profile input. Run a concurrent publish or suspend versus inquiry test and assert transaction ordering. See [testing](testing-strategy.md) and [threat model](security-threat-model.md).
