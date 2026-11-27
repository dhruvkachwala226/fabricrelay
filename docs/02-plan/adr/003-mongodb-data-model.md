# ADR-003 — MongoDB data modeling

## Context

One catalog and bounded colors fit the common read and edit paths.

## Alternatives considered

Separate collections for Catalog and Colorway or relational persistence.

## Proposed decision

Embed Catalog and Colorway. Keep Inquiry and AuditLog separate and use one operational collection.

## Reasoning

Catalog ownership is one-to-one and colors ship with listing edits. Snapshots have a separate lifetime.

## Benefits

Fewer joins and historical inquiry context.

## Drawbacks

Parent document contention and bounded array requirements.

## Consequences

Set limits, reserve archived identifiers and measure query plans. [Detailed design](../database-schema.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
