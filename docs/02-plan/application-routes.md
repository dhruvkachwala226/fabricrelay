# Application routes

Use route groups for layouts with an unchanged external URL structure. Protection is enforced by server loaders and use cases. The layout provides navigation and an early redirect. Use login with a safe relative return path or role home for authenticated visitors. Render access-denied or contact-operator guidance for suspension. Use a generic unavailable page for ineligible public slugs.

| Route | Layout and purpose | Protection |
| --- | --- | --- |
| / | Public landing and sign-in entry | Public |
| /login | Login and fragment-based activation | Public. Authenticated users redirect to role home unless using a valid activation flow. |
| /p/[businessSlug] | Branded catalog, search and filters | Active business, published catalog |
| /p/[businessSlug]/fabric/[fabricSlug] | Fabric details and sample/quote forms inline or in an accessible dialog | Catalog gate plus published tenant listing |
| /admin/dashboard | Platform overview and recent activity | Super Admin |
| /admin/businesses | Business list and create flow | Super Admin |
| /admin/businesses/[id] | Edit business, provision assigned user, change business state | Super Admin and explicit target |
| /admin/users | List provisioned wholesalers and manage their account state | Super Admin |
| /admin/catalogs | Publication metadata and business status | Super Admin, read only |
| /admin/audit-logs | Recent platform activity | Super Admin |
| /dashboard | Tenant overview and primary actions | Active tenant admin |
| /dashboard/fabrics | Draft and listing management | Active tenant admin |
| /dashboard/fabrics/new | Start a draft and attach photos | Active tenant admin |
| /dashboard/fabrics/[id] | Private fabric detail and state | Active tenant admin and owned ID |
| /dashboard/fabrics/[id]/edit | Edit specifications, price, photos and colorways | Active tenant admin and owned ID |
| /dashboard/catalog | Publication controls and copy public URL | Active tenant admin |
| /dashboard/inquiries | Tenant request queue | Active tenant admin |
| /dashboard/inquiries/[id] | Request, contact details and status action | Active tenant admin and owned ID |
| /dashboard/profile | Branding and logo | Active tenant admin |
| /dashboard/settings | Own password change and sign-out | Active tenant admin |

The operational liveness endpoint is GET /api/health. It has no tenant ownership, accepts no input and returns only a generic status. Readiness remains internal.

Desktop admin navigation uses a sidebar. Mobile uses a compact labeled menu and visible page-level primary action. Platform links cover Dashboard, Businesses, Users, Catalogs and Audit logs. Tenant links cover Dashboard, Fabrics, Catalog, Inquiries, Profile and Settings. Catalog pages include breadcrumbs and a back-to-catalog link. See [UI wireframes](ui-ux-plan.md) and [REST contracts](api-contracts.md).
