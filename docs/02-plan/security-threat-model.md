# Security threat model

Trust boundaries include the browser, trusted ingress, authenticated application, database and Cloudinary. Protect credentials, private tenant records, buyer contact, publication state and media. Test only synthetic, isolated fixtures.

| Threat | Potential impact | Preventive control | Verification |
| --- | --- | --- | --- |
| Authentication bypass | Unauthorized access to private pages or mutations | Auth.js cookie and CSRF protection, authoritative account and version checks, same-origin custom mutations, rate-limited credentials | Exercise unauthenticated, foreign-origin, revoked and expired requests. Verify generic login failures. |
| Role escalation | Unauthorized platform powers | Strict role-specific use cases, server-owned role and status, explicit role allowlists | Submit disallowed role/status fields and call each platform route as wholesaler. |
| Cross-tenant access | Business or inquiry disclosure, writes in another tenant | Verified tenant scope and database owner predicates across repositories and media | Two-tenant tests for lists, references, cursors and every write, asserting unchanged foreign records. |
| Insecure direct object references | Guessable resource grants private access | Validate IDs and parent/child ownership, uniform missing/foreign responses | Exercise direct page, API and media URLs with unowned targets and inspect responses. |
| MongoDB injection | Unauthorized filters, updates or data access | Strict primitive Zod schemas, build named predicates server-side, escape search terms, never accept Mongo operators | Validate that object-shaped filters, operator-like keys and invalid scalar values are rejected or treated as literal input. |
| Cross-site scripting | Malicious content executes on catalog or admin pages | Render plain text through React, validate links and apply CSP compatible with media flow | Verify fixture labels and notes render as text. Review CSP, escaping and allowed link schemes. |
| Unsafe uploads | Provider abuse, malicious file delivery or image overwrite | Signed bounded slots, authenticated assets, format and byte verification, no overwrite, authorized delivery and durable cleanup | Exercise unsupported type, oversized file, foreign lease, unconfirmed upload and unauthorized delete/replace. |
| Public endpoint abuse | Spam, write bursts or resource exhaustion | Bounded input, persistent intake quota, idempotency and retry limits | Concurrent tests across app instances, including 11th accepted attempt and repeated request key. |
| Secret exposure | Provider compromise or session forgery | Server-only credentials, environment isolation and redacted build, application and audit logs | Scan committed text, client bundle and logs. Verify sanitized alert payloads and rehearse rotation. |
| Buyer data exposure | Privacy harm and unauthorized contact | Minimal contact capture, restricted detail DTO, no shared cache, retention and private purge | Inspect public/API/list/audit/log shapes and verify tenant-scoped purge on synthetic records. |
| Stale publication or suspension | Private catalog persists after access revoked | Fresh visibility checks on pages, APIs and image delivery, fenced inquiry writes | Reuse URLs and cookies after change, race intake against publication and verify no stale cache hit. |

Residual risks include IP rotation, shared-IP false positives, already-downloaded images and timing races for in-flight reads. Use incident procedures and production launch checks in [deployment](deployment-strategy.md). See [tenant controls](tenant-isolation.md) and [testing](testing-strategy.md).
