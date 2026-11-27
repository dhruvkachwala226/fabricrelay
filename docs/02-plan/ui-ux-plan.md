# Mobile-first UI and UX plan

These text wireframes show primary content in vertical reading order. Brackets denote controls. Desktop uses a two-column layout or table where comparison benefits. All private data shown below is synthetic.

| Screen | Mobile wireframe |
| --- | --- |
| Landing | FabricRelay / Catalogs for textile wholesalers / Short description / [Wholesaler sign-in]. Shared catalog URLs open directly from buyer messages. |
| Login | FabricRelay / Email / Password [Show] / [Sign in] / Operator help. Activation variant: Set password / Confirm password / [Activate account]. |
| Super Admin dashboard | Menu / Dashboard / Business and user counts / [Add business] / Recent activity with actor, tenant and time. |
| Business management | Menu / Businesses / [Add business] / Status filter / Business cards with state. Detail: branding, slug, assigned admin, [Provision or reissue], [Activate or suspend] and impact confirmation. |
| Wholesaler dashboard | Menu / Business name and catalog state / [Add fabric] / [Open catalog settings] / Fabric and inquiry summaries / Recent inquiry shortcuts. |
| Fabric listing | Menu / Fabrics / [Add fabric] / State filter / Cards with image, design code, quality and publication state / [View] [Edit]. |
| Fabric creation | Menu / New fabric / [Add swatch photo] / Thumbnails and individual progress / Quality, design code, GSM and width / MOQ unit and amount / Price visibility and conditional rate / Colorways / [Save draft] / [Publish] with unmet conditions. |
| Colorway management | Fabric name / List of named colors with availability text and photo / [Add color] / Row edit name, availability and [Add or replace photo] / Remove confirmation / Inline save status. |
| Public branded catalog | Logo and business name / Search quality or code / [Filters] for color and availability / Active filter chips / Fabric cards with image, code, specs and eligible price / [More fabrics]. |
| Fabric detail | [Back to catalog] / Swatch gallery / Quality and code / GSM, width, MOQ and rate or request-a-quote label / Named color list with availability / [Request sample] [Request quotation]. |
| Sample request | Selected fabric and code / Listed color / Availability notice / Name / Email or phone toggle and value / Optional note / [Send sample request] / Success with reference. |
| Quotation request | Selected fabric and code / Listed color / Availability notice / Requested meters / MOQ and conditional warning / Name / Email or phone / Optional note / [Request quotation] / Success with reference. |
| Inquiry management | Menu / Inquiries / Request cards with type, fabric and status / [Open] / Details with snapshot and contact / [Change status] / Most recent status update. Add the planned optional type/status filters after core acceptance. |

## Mobile upload

Open the device picker or camera from a single action. Create a draft when the first photo is added and show which fields have been saved. Allow a usable partial draft with photo or specs and show publish requirements in one place. Keep keyboard and touch controls unobstructed by the action footer. Reuse numerical input hints, units adjacent to fields, conditional pricing fields and per-image progress with retry. Ask before leaving with unsaved edits.

## Visual system and state rules

| Element | Plan |
| --- | --- |
| Typography | Use Inter with system fallback. Body 16 px, labels at least 14 px, page headings 24–32 px with medium or bold emphasis. Use readable line height and tabular numerals where quantities align. |
| Spacing | Use a 4 px base and 8, 12, 16, 24, 32 and 48 px steps. Use at least 16 px mobile page padding and aim for 44 px touch targets. |
| Color | CSS tokens for background #F8FAFC, surface #FFFFFF, text #0F172A, muted #475569, primary #0F766E, focus #1D4ED8, danger #B91C1C, success #166534 and warning #92400E. Pair semantic colors with labels and verify actual contrast. Keep tenant branding to logo/name so navigation contrast is predictable. |
| Reuse | App shell, heading/action bar, status label, item card, table, filter drawer, form field, upload tile, empty state, inline error, confirmation dialog and pagination controls. |
| Breakpoints | Base single column, test 360 and 390 px. Use 640 px for two-card layouts where content fits and 1,024 px for a persistent admin sidebar. Let tables become cards or labeled rows on narrow screens. |
| Validation | Give hints on required fields and units. Validate on submit with inline errors and focus the error summary or first invalid field. Preserve valid values. Distinguish nonblocking MOQ or availability notices from field errors. |
| Loading | Use stable-size placeholders for lists and per-image progress. Disable duplicate submits while pending and announce result. Retry with the same request key on failure. |
| Empty | Explain why there are zero items and offer one relevant action. Examples include Add fabric, Clear filters and Share published catalog. |
| Error | Keep typed form state during retryable failures. Handle publication loss with an unavailable page, and rate limits with a retry time. Use generic sign-in errors. |
| Accessibility | Target WCAG 2.2 AA for the core flow. Keyboard-operable controls, visible focus, landmarks, meaningful image alternatives, dialog focus return, programmatic labels and live announcements for errors and success. Use names and status text alongside color swatches. Verify zoom and reduced-motion behavior. |

Wireframes guide planning. Implementation and mobile usability measurement belong to later milestones. See [routes](application-routes.md) and [test coverage](testing-strategy.md). [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
