# ADR-005 — Authentication strategy

## Context

Two administrator roles use manual onboarding.

## Alternatives considered

OAuth, email magic links or custom sessions.

## Proposed decision

Use Auth.js Credentials, manual one-use activation and authoritative session version checks.

## Reasoning

Operator-controlled onboarding avoids an additional provider and fresh checks enforce revocation.

## Benefits

Simple provisioning and next-request suspension.

## Drawbacks

Application owns hashes, resets and login throttling.

## Consequences

Exercise activation, resets and revocation end to end. [Detailed design](../authentication.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
