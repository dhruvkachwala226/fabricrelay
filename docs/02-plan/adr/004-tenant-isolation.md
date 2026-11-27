# ADR-004 — Multi-tenant isolation

## Context

The pilot must protect private data and buyer contacts.

## Alternatives considered

One database per business or platform impersonation.

## Proposed decision

Use shared collections with owner predicates and explicit platform scope.

## Reasoning

Shared storage keeps provisioning small and narrow platform use cases preserve boundaries.

## Benefits

One index and backup strategy with auditable platform access.

## Drawbacks

Missing owner predicates can leak records across tenants.

## Consequences

Require verified context and two-tenant tests. [Detailed design](../tenant-isolation.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
