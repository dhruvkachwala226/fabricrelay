# Deployment and automation strategy

Use GitHub Actions for checks, Vercel Node.js for the web application, Atlas for MongoDB replica-set storage and Cloudinary for media. Keep environments separate. Validate network access, region choice and hosting tiers ahead of production. No provider is configured as part of M02.

| Environment | Plan |
| --- | --- |
| Local | Node.js 24, versioned npm lockfile, gitignored local env, local replica set or explicitly separate development Atlas DB, media adapter fake or isolated media environment. |
| CI and preview | CI ephemeral replica set and synthetic seed data. Use isolated preview Atlas namespace and Cloudinary credentials when a live smoke is needed. Preview uses access controls and no production credentials. Restrict secret-bearing jobs from untrusted pull request code. |
| Production | Vercel Node runtime near the Atlas region, environment-specific DB identity and Cloudinary credentials, HTTPS origin and separately scoped secrets. Reuse a process-level Mongoose connection promise with a bounded pool and short timeouts. Deploy additive indexes before enabling dependent routes. |

| Variable | Use |
| --- | --- |
| MONGODB_URI, MONGODB_DB | Scoped database connection and database name. |
| AUTH_SECRET, APP_ORIGIN | Auth.js secret and canonical HTTPS origin. Configure any Auth.js host trust only for verified deployment ingress. |
| CLOUDINARY_CLOUD_NAME, CLOUDINARY_API_KEY, CLOUDINARY_API_SECRET, CLOUDINARY_UPLOAD_PRESET | Scoped media integration. Only required public upload fields reach the browser. |
| REQUEST_KEY_HMAC_SECRET, RATE_LIMIT_HMAC_SECRET | Separate digest keys for inquiry payload equality and requester privacy. Coordinate rotations with retry and quota windows. |
| OPS_JOB_SECRET | Optional secret for an authenticated scheduled reconciliation handler, if M13 selects that approach. |

Use Vercel secret storage and GitHub Actions environments with scoped permissions and deployment approvals where available. Document only dummy env values in examples. Keep authenticated pages and media visibility responses uncached at the shared layer. Never log secrets, tokens or buyer contact.

## Release and recovery

1. CI uses npm ci, lint, strict type checking, Vitest, Next build and isolated Playwright. Add release evidence, provider smoke and synthetic security/privacy checks before production promotion.
2. Verify that a production liveness endpoint returns only a generic status. Use an internal readiness smoke that checks MongoDB connectivity and expected indexes and a controlled provider smoke using an isolated synthetic draft. Record UTC timing, outcome and deployment ID. Run role and publication checks with designated synthetic tenants. Clean them up through an approved operator procedure.
3. If verification fails, freeze promotion and roll back the Vercel deployment to the last healthy release. Keep data changes additive through the rollback window. Do not roll back data by default. Verify the previous release works with the current schema and review any interrupted writes or media cleanup. Restore Atlas only through an authorized incident decision after assessing the loss window.
4. Run media reconciliation with an authenticated operator command during the pilot. In M13 automate synthetic health and cleanup only after confirming deployment cadence and secret handling. Retain sanitized failure alerts.

## Persistent limits in serverless

Use the same MongoDB operational collection across function instances. Enforce inquiry quota in the durable write transaction. Scoped TTL cleans stale counters asynchronously. Application code checks current timestamps. In-memory limits reset or disagree across instances. A separate Redis service could absorb higher-volume counters but adds another provider and complicates accepting and counting each inquiry atomically. Launch with MongoDB and measure lock contention, database connections and performance before revisiting the design. [Inquiry workflow](inquiry-workflow.md) · [MongoDB transactions](https://www.mongodb.com/docs/manual/core/transactions/) · [Vercel request headers](https://vercel.com/docs/headers/request-headers)

## Open release decisions

Owner to choose buyer-contact retention and backup handling before production. Confirm cost and network access policy for Atlas and provider plans, media guardrails, representative browsers, and staging results. Exact package versions are gated on M03 compatibility verification. Run the approved production checklist in M12. See [implementation plan](implementation-plan.md).
