# Product requirements

These are proposed product acceptance criteria. Tests and measurements are planned for the approved implementation milestones.

## Example listing

The brief supplies a synthetic test fixture. Pure Modal Satin, design code `MS-402`, 80 GSM, width (panna) 58 inches, INR 74 per meter, MOQ 1 lump (100 meters), and Rose, Teal, Olive and Wine colorways. Use test availability values for each color. No market price or stock claim is implied.

## Functional requirements

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| FR-001 | Admin authentication and membership | A provisioned `WHOLESALER_ADMIN` can sign in and access their active business dashboard. Unauthenticated, unassigned and suspended users are denied protected tenant actions. Only `SUPER_ADMIN` and `WHOLESALER_ADMIN` are authenticated roles. |
| FR-002 | Business profile and branding | `SUPER_ADMIN` can create and edit a business profile. Its authorized `WHOLESALER_ADMIN` can change the display name and logo shown on the tenant's public catalog. A wholesaler from another tenant cannot read or update the private profile. |
| FR-003 | Fabric listing and photos | A wholesaler can create a draft, upload at least one fabric photo, and save quality, design code, GSM and width with a unit. Invalid measurements and duplicate design codes in the same tenant are rejected. The fixture displays 80 GSM and 58 inches when published. |
| FR-004 | Colorways and availability | A wholesaler can add, edit or remove named colorways and mark each available or unavailable. The published fixture displays Rose, Teal, Olive and Wine by name. Changing Teal to unavailable appears on the next public reload after a successful save. Existing inquiries retain the color context captured at submission. |
| FR-005 | Rates and MOQ | Store MOQ as a positive quantity and unit, with an optional local label or equivalent. Support a published indicative INR/m rate or a request-a-quote display. The fixture shows 1 lump (100 meters). In quote-only mode, the numeric rate is absent from the public page and payload. |
| FR-006 | Publication and sharing | A tenant admin can publish or unpublish the catalog and its listings. A copied stable public URL opens by HTTPS without login and shows only published records for that business. Drafts and unpublished catalogs remain inaccessible through direct public URLs. |
| FR-007 | Public browsing and filters | A public buyer can search by quality or design code and filter by color and availability. Searching for `MS-402` in the correct catalog finds the published fixture. Incompatible criteria show an accessible empty state with a way to reset filters. |
| FR-008 | Physical sample request | A guest submits a sample request with a valid name, one contact method and a selected listed color. Exactly one request is stored in the catalog tenant, with the fabric and color context, and a reference is shown after success. Invalid input or an unavailable catalog creates no request. A color marked unavailable can be queried, with the status visible to the buyer. |
| FR-009 | Wholesale quotation request | A guest submits the same contact and fabric context plus a positive requested quantity in meters. If a meter-equivalent MOQ exists and the request is smaller, show a below-MOQ notice and record it for review. One quote inquiry and reference are created, with no allocation of stock. |
| FR-010 | Inquiry management | The authorized wholesaler can see its sample and quotation requests, open details and persist the status New, In progress or Closed. Changes remain visible after refresh and a second tenant cannot list, open or change the requests. |
| FR-011 | Super Admin business operations | A `SUPER_ADMIN` can provision the initial wholesaler membership and activate or suspend the business. Suspending blocks tenant-admin actions, public reads and new inquiries on the next request. Reactivation restores access according to membership and publication state. Recent platform activity lists account creation or state changes with actor, business, action and timestamp. A `WHOLESALER_ADMIN` is denied these operations. |
| FR-012 | Inquiry queue convenience | A wholesaler can filter the queue by type and status. Combining Sample and New returns only matching requests in the same tenant and clearing both restores the complete queue. |
| FR-013 | Discovery convenience | A buyer can filter on a GSM range and width. On the fixture, 70–90 GSM and 58 inches match. An incompatible width or range returns an empty state. |

## Non-functional requirements

| ID | Area | Measurable acceptance criteria |
| --- | --- | --- |
| NFR-001 | Security | Server tests reject unauthorized reads and writes, invalid form payloads and non-allowlisted uploads. A file over 5 MB is rejected with a useful error. With a configured public form rate limit of ten accepted requests in ten minutes per requester and catalog, the eleventh returns a throttling response and creates no record. |
| NFR-002 | Tenant isolation | In a two-tenant fixture, tests cover catalog, fabric, colorway, media and inquiry reads and mutations. Attempts with another tenant's ID or record reference return no private data and cause zero cross-tenant writes. Public requests derive scope from the published catalog server-side. |
| NFR-003 | Mobile responsiveness | At 360 px and 390 px CSS viewport widths, catalog, fabric details, both request forms and common wholesaler actions fit without horizontal scrolling and primary controls remain usable. |
| NFR-004 | Accessibility | Aim for WCAG 2.2 AA on core flows. Verify keyboard-only completion, visible focus, labeled inputs, named colorways, meaningful image alternatives and announced errors. Automated checks on catalog, detail, both forms and admin queue report zero serious or critical findings, supplemented by a manual check. [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/) |
| NFR-005 | Performance | On a staging tenant with 100 published fabrics and an agreed mobile profile of 4x CPU slowdown, 1.6 Mbps downstream and 150 ms RTT, median LCP across five cold catalog loads is at most 2.5 seconds. Public listing images use sized variants. |
| NFR-006 | Reliability | A simulated failed inquiry write shows a retryable error and creates zero records. Double submission or retry after the first successful write with the same request token results in one inquiry and the same reference. |
| NFR-007 | Maintainability | The implementation's type check, lint, Vitest and Playwright workflows pass in CI. Critical role, publication and tenant-scope rules have reviewable test cases that cover both allowed and denied access. |
| NFR-008 | Data privacy | Capture name and one contact method plus request context. Sample requests need no shipping address at submission. Inspect public responses, activity and application logs to confirm zero buyer contact values or credentials. Access to inquiries is tenant-scoped, with a documented and tested deletion procedure for pilot records. |

The [MVP scope](mvp-scope.md) defines priority. The [success metrics](success-metrics.md) define proposed pilot outcomes.
