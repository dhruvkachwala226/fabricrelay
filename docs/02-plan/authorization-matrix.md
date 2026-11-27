# Authorization matrix

| Capability | SUPER_ADMIN | WHOLESALER_ADMIN | Guest buyer |
| --- | --- | --- | --- |
| View platform dashboard, business directory and private platform business record | Yes, platform workflow | Denied | Denied |
| Create, edit, activate or suspend business | Yes, audited platform workflow | Denied | Denied |
| List, provision, suspend, reactivate or reissue activation for wholesaler users | Yes, audited platform workflow | Denied | Denied |
| Inspect publication metadata | Yes, platform catalog status view | Own catalog | Published public view |
| View or update wholesale branding and manage logo | Denied through tenant routes | Own active business | Published branding only |
| List drafts, edit fabrics and colorways, manage swatches | Denied through tenant routes | Own active business | Published listing and eligible media only |
| Publish, unpublish or archive catalog or fabric | Denied | Own active business | Denied |
| Submit guest sample or quotation | Public eligibility rules | Public eligibility rules | Public eligibility rules |
| Read queue, see buyer contact or change inquiry status | Denied | Own active business | Denied |
| Read platform audit activity | Yes | Denied | Denied |
| Change own password | Yes through account endpoint | Yes through account endpoint and settings | Denied |

Public eligibility is identical for all visitors. Role assignment is performed only by constrained provisioning logic. Protected platform endpoints never convert a platform principal to a wholesaler principal.

| Helper | Input and outcome |
| --- | --- |
| requireAuth() | Read Auth.js cookie, load user by subject, verify ACTIVE status, version, expiry and archive state. Return a verified principal or fail. |
| requireRole(...allowedRoles) | Call requireAuth and enforce a nonempty allowlist. Return authorized principal or 403. |
| requireTenant() | Require WHOLESALER_ADMIN, load the principal's assigned business, require ACTIVE and unarchived, then return an immutable scope with user and business IDs. |
| requirePublicCatalog(slug) | Validate slug and resolve an active business with published catalog. Return a restricted guest scope. |
| requirePlatformTarget(id) | Require SUPER_ADMIN before resolving any path target. Return a scope limited to the named platform action and target. |

Policies fail closed. Feature use cases accept the narrow verified scope and deny an absent required permission. Apply this on every page loader that reads private data, route handler, and mutation. See [tenant isolation](tenant-isolation.md).
