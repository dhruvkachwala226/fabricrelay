# ADR-001 — Modular monolith architecture

## Context

A solo developer must ship one complete wholesale-to-buyer workflow.

## Alternatives considered

Microservices or an application with unrestricted module access.

## Proposed decision

Use one deployable application with named modules and typed server use cases.

## Reasoning

The main workflows share state, tenant eligibility and release timing.

## Benefits

Simple deployment and shared transactions.

## Drawbacks

Shared failure domain and release cadence.

## Consequences

Review boundaries and keep extraction seams. [Detailed design](../architecture.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
