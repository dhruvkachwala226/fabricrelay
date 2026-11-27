# REST API contracts

App APIs are under /api/v1. Auth.js keeps its own /api/auth protocol. All app JSON successes use `{data: T}` or `{data: T[], page: {nextCursor: string | null}}`. HTTP 204 has no body. Media success is a byte stream. Errors use `{error: {code, message, requestId, fields?}}`. Field errors are path-to-message arrays. Never serialize database objects, provider errors or exception details. Page loaders reuse these use cases and DTO rules.

## Common authorization, validation and errors

**P** requires a verified SUPER_ADMIN and a named platform workflow. **T** requires an active WHOLESALER_ADMIN session and the tenant from requireTenant. **A** allows either authenticated role for their own credentials, with active tenant policy for the wholesaler. **G** requires no session and derives tenant from eligible public slug where present. A valid role is mandatory even when a caller knows the URL. Apply same-origin checks on cookie-authenticated mutations.

Paths use canonical ASCII slugs or strict 24-hex ObjectIds as indicated by variable names. `businessId` in a P path or filter selects a platform target and never supplies Tenant scope. Strict Zod rejects extra keys, invalid ranges and non-JSON write bodies. Bound normal JSON to 32 KiB and public inquiry JSON to 8 KiB. Lists accept an optional opaque cursor bound to scope, filters and ordering and an integer limit of 1–50, default 20. The default sort is newest first by createdAt and ID unless specified. Public fabric list sorts by ID in ascending order. Lookup endpoints and mutations have no pagination. Unless listed, there is no query or body. Empty patches are invalid.

Trim and bound business display names and fabric quality to 1–120 characters, colorway names to 1–60, descriptions to 1,000 and uploaded alt text to 160. Validate canonical email up to 254 characters. Use null to clear optional descriptions or the stored rate. A null indicative rate fails readiness. Validate expected states against the target entity's declared enum and editor versions as nonnegative safe integers. All booleans and numbers use explicit parsers. Do not coerce arbitrary values through JavaScript truthiness.

Apply 400 for malformed input/cursor, 413 for oversized request, 415 for wrong media type, 422 for field or semantic validation failure. Private routes also use 401 for missing/invalid/revoked principal, 403 for policy denial, and 404 for an absent or inaccessible scoped target. G uses 404 for ineligible public resources. Use 409 for conflicts or invalid transition, 429 plus Retry-After for throttling, and 503 for transient store or provider unavailability. An unexpected failure returns generic 500. These common errors apply where relevant. The last column calls out endpoint-specific behavior.

## Auth.js and credential lifecycle

Auth.js handles its own redirects, cookies, CSRF response and error shapes. Use the official client or server integration for these routes. Unknown providers or actions are unavailable. The application applies strict Credentials validation inside the authorize callback. See [authentication](authentication.md) and [protocol reference](https://authjs.dev/reference/nextjs).

| HTTP route | Scope | Params or body | Success and validation | Specific errors |
| --- | --- | --- | --- | --- |
| GET /api/auth/providers | G, global | None | 200 Auth.js configured provider map | 503 |
| GET /api/auth/csrf | G, browser | None | 200 Auth.js CSRF token with its cookie | 503 |
| GET /api/auth/session | G, browser cookie | None | 200 minimal Auth.js session or empty session, no ownership grant | 503 on unavailable dependency |
| POST /api/auth/callback/credentials | G, bound to Auth.js CSRF | Auth.js form token, email, password, optional same-origin callback URL | Auth.js sets session cookie and redirects or returns its client-protocol result | Generic auth failure for invalid credentials or eligibility. Uniform throttle failure and safe callback handling. |
| POST /api/auth/signout | G, current browser cookie and CSRF | Auth.js form token, optional same-origin callback URL | Auth.js clears cookie and redirects or returns its client-protocol result | Auth.js CSRF/redirect errors |
| POST /api/v1/auth/activate | G, token-scoped account | `{token, password}` | 204 consume 256-bit token and set password (12–128 chars) | 422 generic invalid or expired token, 429 |
| POST /api/v1/account/password | A, current user | `{currentPassword, newPassword}` | 204 verify current password and revoke existing sessions | 422 invalid password, 429 |

Any provider-managed sign-in/error page redirects into the custom login flow or uses the Auth.js protocol. No application registration endpoint.

## Platform operations

| HTTP route | Scope | Params or body | Success | Specific validation or errors |
| --- | --- | --- | --- | --- |
| GET /api/v1/admin/dashboard | P, platform | None | 200 `{businessCounts, userCounts, recentActivity}` | Activity capped at 10 events |
| GET /api/v1/admin/businesses | P, platform | Page, optional business status | 200 paged BusinessSummary | Validate enum |
| POST /api/v1/admin/businesses | P, platform | `{displayName, slug, description?}` | 201 BusinessDetail, INACTIVE/DRAFT | 409 slug unavailable |
| GET /api/v1/admin/businesses/[businessId] | P, target | ID | 200 BusinessDetail | 404 missing |
| PATCH /api/v1/admin/businesses/[businessId] | P, target | ID, subset of `{displayName, slug, description}` | 200 BusinessDetail | 409 locked or taken slug |
| PATCH /api/v1/admin/businesses/[businessId]/status | P, target | ID, `{status}` ACTIVE or SUSPENDED | 200 BusinessDetail | 409 archived or prohibited change |
| GET /api/v1/admin/users | P, platform | Page, optional businessId and user status | 200 paged UserSummary | Results limited to wholesaler accounts |
| POST /api/v1/admin/businesses/[businessId]/users | P, target | ID, `{email}` | 201 UserSummary and one-time activation URL | 404 missing business, 409 duplicate email or archived business |
| PATCH /api/v1/admin/users/[userId]/status | P, explicit wholesaler | ID, `{status}` ACTIVE or SUSPENDED | 200 UserSummary | 409 pending activation or invalid state |
| POST /api/v1/admin/users/[userId]/activation | P, explicit wholesaler | ID | 201 one-time activation URL | 409 suspended or archived user, 429. For an ACTIVE account this is an operator reset. |
| GET /api/v1/admin/catalogs | P, platform | Page, optional catalog state | 200 paged `{businessId, displayName, slug, businessStatus, catalogState, publishedAt?}` | Enum validation |
| GET /api/v1/admin/audit-logs | P, platform | Page, optional businessId | 200 paged AuditEvent | Namespaced allowlisted action and metadata |

BusinessSummary is `{id, displayName, slug, status, catalogState, createdAt}`. BusinessDetail adds `description?, updatedAt`. UserSummary is `{id, email, role, businessId, status, createdAt}`. AuditEvent is `{id, actorUserId, businessId?, action, targetType, targetId, createdAt, metadata?}`. Platform privilege never returns raw activation digests, passwords, buyer records or private media. Activation secrets appear only in the one-time response, with no-store and log redaction.

## Wholesaler operations

| HTTP route | Scope | Params or body | Success | Specific validation or errors |
| --- | --- | --- | --- | --- |
| GET /api/v1/dashboard | T, own | None | 200 `{fabricCounts, inquiryCounts, catalogState}` | Counts scoped to authorized tenant |
| GET /api/v1/profile | T, own | None | 200 Profile | Active business required |
| PATCH /api/v1/profile | T, own | Subset of `{displayName, description}` | 200 Profile | Logo through scoped media routes |
| GET /api/v1/fabrics | T, own | Page, optional state | 200 paged FabricSummary | Validate state |
| POST /api/v1/fabrics | T, own | Partial FabricInput, optional draft slug | 201 FabricDetail | 409 duplicate slug/design code. Generate a unique draft slug when omitted. |
| GET /api/v1/fabrics/[fabricId] | T, owned fabric | ID | 200 FabricDetail | 404 foreign/missing |
| PATCH /api/v1/fabrics/[fabricId] | T, owned fabric | ID, `{version, ...FabricInput}` with editable subset | 200 FabricDetail | 409 stale, archived or duplicate. Published listings must stay ready. |
| PATCH /api/v1/fabrics/[fabricId]/publication | T, owned fabric | ID, `{version, state}` PUBLISHED, UNPUBLISHED or ARCHIVED | 200 FabricDetail | 409 stale or invalid transition, 422 missing publish fields |
| GET /api/v1/fabrics/[fabricId]/colorways | T, owned fabric | ID | 200 Colorway[] bounded by schema | Active colors only |
| POST /api/v1/fabrics/[fabricId]/colorways | T, owned fabric | ID, `{version, name, availability?}` | 201 `{colorway, fabricVersion}` | 409 stale or duplicate, 422 cap reached |
| PATCH /api/v1/fabrics/[fabricId]/colorways/[colorwayId] | T, owned fabric and child | IDs, `{version, name?, availability?}` | 200 `{colorway, fabricVersion}` | 404 missing/foreign, 409 stale or duplicate |
| DELETE /api/v1/fabrics/[fabricId]/colorways/[colorwayId] | T, owned fabric and child | IDs, query version | 204 detach and archive | 409 stale or removal would invalidate published listing |
| GET /api/v1/catalog | T, own | None | 200 `{state, publicUrl, firstPublishedAt?, publishedAt?, unpublishedAt?, archivedAt?}` | Canonical origin |
| PATCH /api/v1/catalog/publication | T, own | `{state, expectedState}` | 200 catalog DTO | 409 stale or invalid transition, 422 no publish-ready listing |
| GET /api/v1/inquiries | T, own | Page, optional type and status if enabled | 200 paged InquirySummary | Reject filters until optional scope enabled |
| GET /api/v1/inquiries/[inquiryId] | T, owned inquiry | ID | 200 InquiryDetail, no-store | 404 foreign/missing |
| PATCH /api/v1/inquiries/[inquiryId]/status | T, owned inquiry | ID, `{status, expectedStatus}` | 200 InquirySummary | 409 transition conflict |

Profile is `{displayName, slug, description?, logo?}`. FabricInput allows `slug?, quality?, designCode?, description?, widthInches?, gsm?, moq?, priceVisibility?, ratePaisePerMeter?` with the [schema limits](database-schema.md). Clearing optional values uses explicit null only on a documented nullable patch field. FabricSummary is `{id, slug, quality?, designCode?, state, updatedAt, version}`. FabricDetail adds the remaining FabricInput fields plus `photos, colorways`. Colorway is `{id, name, availability, photo?}`. Media descriptors contain app-owned opaque image ID, authorized view URL and alt text. InquirySummary is `{id, reference, type, status, designCode, colorwayName, belowMoq, createdAt}` from snapshot. InquiryDetail adds `buyerName, contact, note?, requestedMeters?, snapshot, statusChangedAt?`.

## Media and guest operations

| HTTP route | Scope | Params or body | Success | Specific validation or errors |
| --- | --- | --- | --- | --- |
| GET /api/health | G, global operational exception to /api/v1 | None | 200 `{status: "ok"}` | Generic liveness only. Internal readiness is separate. |
| POST /api/v1/media/uploads | T, own target | `{target, file: {bytes, format}, replacesMediaId?}` | 201 `{uploadId, uploadUrl, signedFields, expiresAt}` | 413 exceeds 5 MB, 422 unsupported file/target, 409 capacity, 429 quota |
| POST /api/v1/media/uploads/[uploadId]/confirm | T, own lease and parent | ID, optional `{alt}` | 200 attached media descriptor | 409 expired, superseded or incomplete upload, 422 metadata mismatch |
| DELETE /api/v1/media/[mediaId] | T, own reference | Opaque ID | 204 detach and durably queue deletion | 404 foreign/missing, 409 removal breaks readiness |
| GET /api/v1/media/[mediaId] | T, own reference | ID, optional size enum | 200 optimized image bytes | 404 foreign/missing, 503 delivery failure |
| GET /api/v1/public/[businessSlug] | G, eligible business | Slug | 200 PublicCatalog | 404 when unavailable |
| GET /api/v1/public/[businessSlug]/fabrics | G, eligible catalog | Page, `q?, color?, availability?`, optional `gsmMin?, gsmMax?, widthInches?` when enabled | 200 paged PublicFabric cards | 422 invalid range or filter. q max 64 chars. |
| GET /api/v1/public/[businessSlug]/fabrics/[fabricSlug] | G, eligible listing | Slugs | 200 PublicFabric detail | 404 unpublished/foreign |
| GET /api/v1/public/[businessSlug]/media/[mediaId] | G, eligible parent | Slug, opaque ID, optional size enum | 200 optimized image bytes | 404 ineligible reference, 503 delivery failure |
| POST /api/v1/public/[businessSlug]/fabrics/[fabricSlug]/sample-requests | G, eligible listing | Slugs, Idempotency-Key header, SampleInput | 201 `{reference}`, 200 for identical retry | 409 key reused with changed payload, 422 invalid selected color, 429 |
| POST /api/v1/public/[businessSlug]/fabrics/[fabricSlug]/quotation-requests | G, eligible listing | Slugs, Idempotency-Key header, SampleInput plus `requestedMeters` | 201 `{reference}`, 200 for identical retry | Sample errors plus 422 if quantity is missing or invalid. Below MOQ is accepted. |

Media target is a strict union of `{kind: PROFILE_LOGO}`, `{kind: FABRIC_PHOTO, fabricId}` and `{kind: COLORWAY_PHOTO, fabricId, colorwayId}`. Size is THUMB, CARD or DETAIL, default CARD. PublicCatalog is `{slug, displayName, description?, logo?}`. PublicFabric card has `{slug, quality, designCode, widthInches, gsm, moq, priceVisibility, ratePaisePerMeter?, thumbnail, colorways}`. Detail adds `description?, photos`. Public colorway is `{key, name, availability, photo?}`, where key is a scoped opaque selector and stable for that color. Public media fields contain only relative app URL and alt text. Remove the rate entirely in quote-only mode.

SampleInput is `{buyerName, contact: {kind: EMAIL | PHONE, value}, colorwayKey, note?}`. Trim names (1–100 chars), validate email or normalize international phone, cap notes at 500 chars and require an opaque 128-bit or stronger request key. Use distinct routes to fix the inquiry type. Reject foreign or archived colors and accept listed unavailable ones with a notice in the UI. Validate quote meters as a decimal string. See [workflow semantics](inquiry-workflow.md) and [media lifecycle](image-upload-architecture.md).
