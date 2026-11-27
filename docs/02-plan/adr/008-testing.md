# ADR-008 — Testing strategy

## Context

Scope and workflow errors need detection without high maintenance.

## Alternatives considered

End-to-end-only tests or completely mocked storage.

## Proposed decision

Use Vitest domain and replica-set integration suites plus Playwright.

## Reasoning

Each suite catches distinct errors, including concurrent persistence.

## Benefits

Fast rule feedback and representative workflows.

## Drawbacks

Browser and replica-set upkeep.

## Consequences

Use isolated synthetic test environments and limited provider smoke. [Detailed design](../testing-strategy.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
