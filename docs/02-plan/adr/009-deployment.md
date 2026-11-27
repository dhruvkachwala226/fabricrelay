# ADR-009 — Deployment architecture

## Context

Keep Vercel, Atlas and Cloudinary operations realistic for one maintainer.

## Alternatives considered

Self-managed hosting, Redis or in-memory quotas.

## Proposed decision

Use GitHub Actions verification and Vercel Node deployment, with persistent quotas in Atlas.

## Reasoning

Serverless instances need durable shared limits and transaction coupling.

## Benefits

Small provider footprint and repeatable deployment checks.

## Drawbacks

Network, connection and provider-tier limits.

## Consequences

Separate environments, use additive data changes and verify rollback. [Detailed design](../deployment-strategy.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
