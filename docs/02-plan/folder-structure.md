# Planned folder structure

```text
src/
  app/
    (public)/                    landing, login and p/[businessSlug]
    (platform)/admin/            platform shell and pages
    (tenant)/dashboard/          wholesaler shell and pages
    api/                         Auth.js and explicit REST adapters
  components/                    shared presentation and generated UI primitives
  modules/
    identity/                    sessions, provisioning and authorization
    businesses/                  platform lifecycle and tenant branding
    fabrics/                     fabric rules and colorway edits
    catalog/                     publication, public reads and DTOs
    inquiries/                   intake, snapshots, queue and status transitions
    media/                       upload authorization, attachment and reconciliation
    audit/                       structured platform activity
  server/
    db/                          connection, indexes and transaction support
    integrations/                Cloudinary client and protected configuration
    security/                    rate limits, same-origin policy and request context
tests/
  unit/
  integration/
  e2e/
  fixtures/
```

Place Zod request schemas and DTO definitions with the owning module. Keep repositories and models server-only. Shared presentation may import DTO types and client-safe helpers. Pages and handlers depend on use cases. Domain code has no dependency on Next.js, Mongoose or Cloudinary. Use direct functions and named repositories before adding abstraction. The structure describes future files. See [architecture](architecture.md) and [application routes](application-routes.md).
