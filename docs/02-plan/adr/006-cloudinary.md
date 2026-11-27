# ADR-006 — Cloudinary image management

## Context

Phone swatches can exceed the standard function request cap and drafts must stay private.

## Alternatives considered

Multipart upload through Vercel or publicly deliverable assets with random IDs.

## Proposed decision

Use direct signed uploads and authenticated assets with app-checked delivery.

## Reasoning

Bytes bypass function request cap while delivery evaluates live visibility.

## Benefits

Private drafts and optimized image variants.

## Drawbacks

Provider constraints and delivery overhead need measurement.

## Consequences

Gate production on media smoke, mobile LCP and cleanup checks. [Detailed design](../image-upload-architecture.md)

## Approval status

Approved as part of the reviewed M02 documentation plan on 2026-11-27. See the [human decision record](../../../evidence/human-decisions.md).
