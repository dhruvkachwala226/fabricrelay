# ADR-002 — Next.js App Router

## Context

Public, platform and tenant surfaces share the same release.

## Alternatives considered

Pages Router or a separate SPA and API.

## Proposed decision

Use route groups, server-rendered pages and explicit REST handlers.

## Reasoning

Route groups provide role layouts and Server Components can call common query services.

## Benefits

One routing and rendering stack.

## Drawbacks

Framework runtime and cache rules need care.

## Consequences

Keep stateful work on Node and test live authorization at each entry point. [Detailed design](../application-routes.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
