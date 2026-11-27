# Implementation plan

M02 ends with documentation and review evidence. The next authorized work follows the four-week sequence in [PLANS.md](../../PLANS.md). Build one complete publish-to-inquiry loop before optional filters. Run unit and integration checks with each vertical slice and use M09 to consolidate regression coverage. M10 gates release security. M13 automates jobs already needed for the pilot.

## Requirements traceability

| ID | Design and verification | Milestone |
| --- | --- | --- |
| FR-001 | [Credentials, activation and scoped sessions](authentication.md). Login, unassigned and suspended cases. | M04 |
| FR-002 | [Platform business and tenant profile APIs](api-contracts.md). Brand in public DTO, deny another tenant. | M05–M07 |
| FR-003 | [Fabric fields and indexes](database-schema.md) and [photos](image-upload-architecture.md). MS-402, unit validation and duplicate code. | M07 |
| FR-004 | [Embedded colorways](database-schema.md). Show all fixture names, persist availability and keep historical snapshots. | M07–M08 |
| FR-005 | [Textile and rate fields](database-schema.md). Verify lump conversion and quote-only omission. | M07–M08 |
| FR-006 | [Publication and canonical URL](catalog-lifecycle.md). Test copied link, direct draft URL, suspension and reload. | M08 |
| FR-007 | [Public REST search](api-contracts.md) and [catalog wireframe](ui-ux-plan.md). Verify design code, color, availability and empty reset. | M08 |
| FR-008 | [Sample workflow](inquiry-workflow.md). Listed unavailable color allowed, one stored record, no address. | M08 |
| FR-009 | [Quotation workflow](inquiry-workflow.md). Positive meters, flag below MOQ and no stock allocation. | M08 |
| FR-010 | [Queue and statuses](inquiry-workflow.md). Map M01 In progress to CONTACTED and Closed to COMPLETED or REJECTED. Verify tenant access and status persistence. | M08 |
| FR-011 | [Platform permissions](authorization-matrix.md) and [auth lifecycle](authentication.md). Provision, suspend, reactivate and review activity. | M04–M05, M08 |
| FR-012 | [Optional queue filters](api-contracts.md). Verify combined and cleared filters if delivered after Must Have. | M08–M09 stretch |
| FR-013 | [Optional measurement filters](api-contracts.md). Verify fixture match and empty state if delivered after Must Have. | M08–M09 stretch |
| NFR-001 | [Threat model](security-threat-model.md), [5 MB media](image-upload-architecture.md) and [persistent limits](inquiry-workflow.md). Verify 11th accepted attempt is rejected. | M07–M10 |
| NFR-002 | [Two-tenant isolation checks](tenant-isolation.md) across all owned resources. | M04–M10 |
| NFR-003 | [Mobile layouts](ui-ux-plan.md) and 360/390 px browser checks. | M06–M09 |
| NFR-004 | [Accessibility tests](testing-strategy.md). Automated and manual keyboard checks. | M09 |
| NFR-005 | [100-fabric mobile performance profile](testing-strategy.md). Median LCP at most 2.5 seconds. | M09, M12 |
| NFR-006 | [Durable inquiry idempotency and retry](inquiry-workflow.md). Failed write produces no record. | M08–M09 |
| NFR-007 | [Quality gates and deployment](deployment-strategy.md). Type check, lint, Vitest and Playwright in CI. | M03, M09, M12 |
| NFR-008 | [Buyer data and purge](inquiry-workflow.md) and [privacy tests](security-threat-model.md). Restrict contact, redact logs and test synthetic purge. | M08–M10 |

## Owner approval and open decisions

The owner approved implementation of the reviewed M02 documentation plan on 2026-11-27. That approval applies to the reviewed architecture and proposed interpretations as a package. It does not record separate votes or authorize development. The [human decision log](../../evidence/human-decisions.md) records what the owner said.

| Approved review item | Where captured |
| --- | --- |
| Modular monolith, embedded catalog/colorways and shared Atlas tenancy | [Architecture](architecture.md), [ER diagram](entity-relationship.md) |
| Status mapping, advisory MOQ and unavailable listed colors | [Inquiry workflow](inquiry-workflow.md) |
| Stable slugs after publication, read-only platform catalog view and own credentials settings | [Lifecycle](catalog-lifecycle.md), [roles](authorization-matrix.md) |
| Manual account activation, signed Cloudinary uploads and protected delivery | [Auth](authentication.md), [media](image-upload-architecture.md) |
| MongoDB-backed limit and four-week build sequence | [Deployment](deployment-strategy.md), [milestones](../../PLANS.md) |

The owner still needs to choose the production buyer-contact retention period and backup handling before M12. M03 will establish exact dependency versions and mobile browser floor. M07 and M12 must verify Cloudinary policy and provider-tier constraints, including size admission and authenticated transformations. M09/M12 must verify mobile LCP with protected image delivery. Customer adoption, pricing norms and inquiry usability await field validation. The [ADR index](adr/README.md) summarizes the reviewed architecture alternatives.
