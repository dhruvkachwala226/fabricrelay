# FabricRelay milestone plan

## M00 — Project initialization

Established repository documentation, agent guidance and the challenge evidence pack. See [M00 evidence](evidence/milestones/M00.md).

## M01 — Understand and problem validation

Documented the problem, workflows, requirements, alternatives, proposed MVP and research protocol. Documentation is complete. Customer validation remains pending. See [M01 evidence](evidence/milestones/M01.md).

## M02 — Architecture and technical planning

Documented the approved architecture proposal, requirements traceability and implementation sequence. See the [architecture](docs/02-plan/architecture.md), [implementation plan](docs/02-plan/implementation-plan.md) and [M02 evidence](evidence/milestones/M02.md).

## Implementation sequence

Week 1 covers M03–M05, week 2 M06–M07, week 3 M08–M09, and week 4 M10–M14. These are solo-developer timeboxes. Ship the Must Have flow before either Should Have filter enhancement. Verification commands below are future commands or named checks to establish during implementation. They do not indicate execution in M02. Each milestone requires its own authorization before work begins.

### M03 — Application foundation

Status is Pending.

- **Objective** — Establish the runnable baseline.
- **Dependencies** — M02.
- **Implementation tasks** — Pin compatible packages and Node version. add app shell, strict TypeScript, env validation and check scripts.
- **Expected deliverables** — Version lockfile, framework shell and CI baseline.
- **Acceptance criteria** — Clean install, lint, type check and build work on local and CI Node.
- **Verification commands** — `npm ci`, `npm run lint`, `npm run typecheck`, `npm run build`.
- **Risks** — Peer incompatibility. pin and verify a supported pairing.
- **Evidence requirements** — Version matrix, commands and build summary. [M03 evidence](evidence/milestones/M03.md).

### M04 — Authentication and tenant security

Status is Pending.

- **Objective** — Prove authoritative authentication and scope.
- **Dependencies** — M03.
- **Implementation tasks** — Implement provisioning, activation, Auth.js, revocation, owner-scoped repository conventions and two-tenant fixtures.
- **Expected deliverables** — Identity module and scoped integration coverage.
- **Acceptance criteria** — Login, role, tenant and suspension checks pass with existing sessions.
- **Verification commands** — `npm run test:unit`, `npm run test:integration`.
- **Risks** — Stale claims or lockout. database checks and controlled recovery.
- **Evidence requirements** — Synthetic role and revocation test results. [M04 evidence](evidence/milestones/M04.md).

### M05 — Super Admin

Status is Pending.

- **Objective** — Enable audited business and account management.
- **Dependencies** — M04.
- **Implementation tasks** — Build dashboard and directories, activate or suspend businesses, provision wholesalers and show recent audit activity.
- **Expected deliverables** — Platform routes and audit workflow.
- **Acceptance criteria** — Operator can onboard a tenant and lifecycle changes create events. Wholesaler access denied.
- **Verification commands** — `npm run test:integration`, `npm run test:e2e`.
- **Risks** — Partial provisioning. retry safely and couple audit writes.
- **Evidence requirements** — Sanitized onboarding and audit verification. [M05 evidence](evidence/milestones/M05.md).

### M06 — Wholesaler dashboard

Status is Pending.

- **Objective** — Provide a usable private workspace.
- **Dependencies** — M04, M05.
- **Implementation tasks** — Set up navigation, dashboard, profile name/description and settings. Integrate logo handling when M07 media is available.
- **Expected deliverables** — Tenant routes, responsive workspace and profile form.
- **Acceptance criteria** — Wholesaler manages own approved fields and cannot see private foreign state.
- **Verification commands** — `npm run test:integration`, `npm run test:e2e`.
- **Risks** — Overbuild of summary screens. keep to basic counts and actions.
- **Evidence requirements** — Synthetic mobile viewport and authorization checks. [M06 evidence](evidence/milestones/M06.md).

### M07 — Fabric and colorway management

Status is Pending.

- **Objective** — Enable mobile drafts, swatches and colorways.
- **Dependencies** — M06.
- **Implementation tasks** — Implement fabric CRUD/archive, bounded embedded colorways, money and MOQ inputs, Cloudinary workflow and listing readiness.
- **Expected deliverables** — Fabric forms, signed media workflow, logo upload and tests.
- **Acceptance criteria** — Synthetic MS-402 round-trips. Cross-tenant or oversized media cannot attach.
- **Verification commands** — `npm run test:unit`, `npm run test:integration`, `npm run test:e2e`.
- **Risks** — Mobile uploads and orphan cleanup. limit formats and validate retries.
- **Evidence requirements** — Mobile upload, ownership, size and archive results. [M07 evidence](evidence/milestones/M07.md).

### M08 — Public catalog and inquiries

Status is Pending.

- **Objective** — Complete the publishing-to-inquiry loop.
- **Dependencies** — M07.
- **Implementation tasks** — Add catalog lifecycle, public DTOs and filters, share link, two intake forms, durable rate limits and queue/status actions.
- **Expected deliverables** — Guest browsing and intake plus wholesaler queue.
- **Acceptance criteria** — Guest sees published active resources only. Sample and quote are stored once and managed by the owner.
- **Verification commands** — `npm run test:integration`, `npm run test:e2e`.
- **Risks** — Serverless concurrency or UI delay. verify persisted rate limits and keep optional filters conditional.
- **Evidence requirements** — Full guest loop, snapshot and idempotency results. [M08 evidence](evidence/milestones/M08.md).

### M09 — Testing

Status is Pending.

- **Objective** — Verify full MVP regression coverage.
- **Dependencies** — M04–M08.
- **Implementation tasks** — Consolidate Vitest/Playwright suites, run mobile, accessibility and performance fixture checks.
- **Expected deliverables** — CI results, synthetic fixtures and measured release targets.
- **Acceptance criteria** — Critical workflow and security coverage passes. Mobile and accessibility checks are recorded and performance meets threshold in staging.
- **Verification commands** — `npm run test:unit`, `npm run test:integration`, `npm run test:e2e`, `npm run build`.
- **Risks** — Flaky browser or staging differences. stable fixtures and diagnosed failures.
- **Evidence requirements** — Sanitized CI and measurement reports. [M09 evidence](evidence/milestones/M09.md).

### M10 — Security review

Status is Pending.

- **Objective** — Verify access and privacy boundaries before shipping.
- **Dependencies** — M09.
- **Implementation tasks** — Review threat mitigations, reproduce defensive checks in isolated fixtures, rehearse tenant purge and fix release blockers.
- **Expected deliverables** — Threat-to-verification report and resolved blockers.
- **Acceptance criteria** — Tenant isolation, suspension, public disclosure and media controls pass. Remaining risks documented.
- **Verification commands** — `npm run test:security`, `npm run test:integration`.
- **Risks** — Cross-tenant shortcuts and media capability leakage. table-driven denial checks.
- **Evidence requirements** — Review results and test artifacts with synthetic data. [M10 evidence](evidence/milestones/M10.md).

### M11 — Documentation

Status is Pending.

- **Objective** — Prepare operating and user guidance.
- **Dependencies** — M10.
- **Implementation tasks** — Document onboarding, publication, inquiries, environment setup, deletion and release recovery.
- **Expected deliverables** — Operator/user guide and release runbook.
- **Acceptance criteria** — Guides match verified behavior and links pass.
- **Verification commands** — `git diff --check`, `run internal Markdown link check`.
- **Risks** — Documentation drift. verify with a walkthrough.
- **Evidence requirements** — Guide inventory and walkthrough evidence. [M11 evidence](evidence/milestones/M11.md).

### M12 — Production deployment

Status is Pending.

- **Objective** — Ship a verified release after owner authorization.
- **Dependencies** — M10, M11.
- **Implementation tasks** — Configure isolated production services, provision authorized identities, verify health and smoke, and rehearse rollback.
- **Expected deliverables** — Production release with recorded smoke and rollback outcome.
- **Acceptance criteria** — HTTPS, authorized health checks, tenant/public smoke and rollback verified. Owner retention decision recorded.
- **Verification commands** — `npm run build`, `curl --fail --silent <approved-health-url>`, `run authorized production smoke`.
- **Risks** — Provider limits, connectivity and retention decision. validate preview first.
- **Evidence requirements** — Deployment ID, UTC check times and sanitized smoke summary. [M12 evidence](evidence/milestones/M12.md).

### M13 — Automation

Status is Pending.

- **Objective** — Automate repeatable operational checks.
- **Dependencies** — M12.
- **Implementation tasks** — Schedule lightweight health and media reconciliation. document retries, alerts and job authorization.
- **Expected deliverables** — Verified schedules and operator instructions.
- **Acceptance criteria** — One successful run and one safe failure/retry exercise are recorded.
- **Verification commands** — `Run authorized workflow dispatch`, `inspect scheduled job result`.
- **Risks** — Cron cost/cadence and credential scope. keep operator fallback.
- **Evidence requirements** — Job configuration references and sanitized outcomes. [M13 evidence](evidence/milestones/M13.md).

### M14 — Final submission

Status is Pending.

- **Objective** — Deliver complete challenge materials.
- **Dependencies** — M12, M13.
- **Implementation tasks** — Reconcile requirements, measured evidence and remaining limitations. produce final submission package.
- **Expected deliverables** — Submission checklist and milestone evidence index.
- **Acceptance criteria** — Claims trace to real artifacts and customer-validation gaps remain explicit.
- **Verification commands** — `git diff --check`, `run internal Markdown and evidence checks`.
- **Risks** — Gaps between claims and evidence. cite verified artifacts only.
- **Evidence requirements** — Submission snapshot, checklist and final verification. [M14 evidence](evidence/milestones/M14.md).
