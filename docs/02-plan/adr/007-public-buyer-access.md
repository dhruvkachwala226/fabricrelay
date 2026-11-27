# ADR-007 — Public buyer access

## Context

Buyers open forwarded wholesale links and request samples or quotations.

## Alternatives considered

Buyer accounts, shared tokens or a public marketplace.

## Proposed decision

Use public published links and allowlisted DTOs with bounded guest intake.

## Reasoning

The MVP needs quick access with tenant isolation and private contact data.

## Benefits

Stable sharing and minimal contact capture.

## Drawbacks

Links can be forwarded and guest requests can attract spam.

## Consequences

Lock slugs after publication, check eligibility and persist idempotency and quota. [Detailed design](../inquiry-workflow.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
