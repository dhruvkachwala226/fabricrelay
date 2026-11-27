# Sample and quotation inquiries

The two forms share buyer name, one contact method, a required listed color, an optional short note and an idempotency key. A sample asks for no quantity or delivery address. A quotation additionally requires positive requested meters. Both are guest flows on a published listing. A listed unavailable color remains selectable with a notice. Reject a missing, archived or foreign color. Compare quotation meters with the exact meter equivalent when available. Below-MOQ quantities display a notice and persist a flag. Revalidate from stored values when submitting. Inquiry submission never reserves stock or fixes a negotiated price.

| Current | Allowed next |
| --- | --- |
| NEW | CONTACTED or REJECTED |
| CONTACTED | COMPLETED or REJECTED |
| COMPLETED | Terminal |
| REJECTED | Terminal |

A successful update stores responsible user and time. A retry of the same state is idempotent. Use a compare-and-set condition for conflicting simultaneous updates. The wholesaler uses existing channels to contact the buyer. Completion or rejection closes the request.

## Intake concurrency and retries

1. Parse strict bounded input. Resolve current active business, published catalog and published fabric. Derive requester identity from the trusted platform IP header and a server HMAC scoped to the business. Use a separate digest for canonical validated payload equality.
2. In a MongoDB transaction, recheck eligibility and resolve the listed color. If the tenant-scoped key exists, return its original reference for the same request payload or 409 for a changed payload. A retry after visibility is revoked receives the generic unavailable response.
3. Check the persisted rolling counter and exclude timestamps outside the prior ten minutes. Return 429 with Retry-After when ten accepted requests are present across both inquiry types. For a new valid write, append one timestamp to that counter and insert the inquiry in the same transaction. TTL only cleans stale counters. Check expiry in application logic and deny intake with 503 if persistence or trusted requester identity is unavailable.
4. Serialize intake against catalog and listing changes by incrementing guarded intake fences on Business and Fabric in that transaction. Recheck eligibility on transaction retry. Keep these writes separate from editor versions and disable timestamp bumps for fence-only changes. Use bounded retry, a transient error on exhaustion and majority acknowledgement. On an ambiguous commit or duplicate-key race, re-read the tenant request key before retrying. Return success only after durable confirmation.

The snapshot includes quality, design code, measurements, selected color and availability, MOQ unit and equivalent when present, and indicative paise per meter only if public at submission. An opaque reference is the sole public response identifier. Keep contact in private inquiry detail, redact it from logs and audit, and remove pilot records through an operator-run, tenant-scoped purge. The owner still needs to select the production retention period and backup handling. Rehearse purge on synthetic data. See [schema](database-schema.md), [API](api-contracts.md) and [testing](testing-strategy.md).
