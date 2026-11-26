# FabricRelay agent instructions

## Project mission

Build a multi-tenant B2B textile catalog and wholesale inquiry management platform. Wholesalers will digitize swatches, manage textile specifications, colorways, prices and availability, and publish branded catalogs. Public buyers will browse published fabrics and request physical samples or wholesale quotations.

## Technology stack

- Application and UI — Next.js App Router, TypeScript, Tailwind CSS, shadcn/ui
- Data and media — MongoDB, Mongoose, Cloudinary
- Authentication and validation — Auth.js, Zod
- Verification — Vitest, Playwright
- CI and hosting — GitHub Actions, Vercel

## User roles

- `SUPER_ADMIN` is the platform administration role. Any cross-tenant operation requires an explicit, server-authorized platform workflow.
- `WHOLESALER_ADMIN` manages the catalog and inquiries of their authorized tenant.
- Public buyers are unauthenticated. They can access the intended published catalog and submit sample or quotation inquiries through public endpoints.

## Engineering standards

- Read project documentation before implementation.
- Work on only one approved milestone at a time.
- Never implement features outside approved scope.
- Prefer the simplest change that satisfies the approved acceptance criteria. Reuse established application patterns and preserve behavior outside the change.
- Handle failure paths and retries so catalog and inquiry records stay consistent.
- Record material decisions and open limitations in the milestone evidence.

## Coding conventions

- Use strict TypeScript, descriptive names, and explicit types at application boundaries. Validate untrusted inputs with Zod when implementing application features.
- Keep database, session, privileged media operations, and secrets in server-only code. Keep client components focused on interactive behavior.
- Use established file and export conventions once the app is scaffolded. Keep rendering, domain rules, and persistence responsibilities clear.

## Security requirements

- Enforce server-side authorization.
- Never commit credentials or private customer data.
- Validate inputs at server entry points. Apply access checks before reading or changing protected data and limit public responses to fields intended for disclosure.
- Keep secrets in environment variables or approved secret stores. Use dummy values in checked-in examples and redact sensitive information from errors, logs, fixtures, and evidence.
- Design public inquiry and upload entry points with abuse controls and bounded input sizes when those features are approved.

## Multi-tenant isolation rules

- Never trust client-supplied tenant IDs.
- Derive administrative tenant scope on the server from the verified session and authorized membership. For public requests, resolve the tenant server-side from the public catalog identifier and enforce publication rules.
- Scope tenant-owned reads and mutations by the authorized tenant alongside the record identifier. Apply the same scope to related records, media, exports, and cached data.
- Deny by default when tenant scope is missing or ambiguous. Treat explicit platform administration as a separately authorized path and test both allowed and denied access.

## Testing requirements

- Verify changes through their normal entry points. Run the relevant project checks and record actual results or the reason a check could not run.
- Use Vitest for unit or integration coverage and Playwright for end-to-end workflows when the tooling exists.
- Cover authorization boundaries, cross-tenant access attempts, publication visibility, and invalid inputs when the affected behavior is implemented or changed.
- Use synthetic fixtures. For documentation-only changes, check structure, links, and repository hygiene as applicable.

## Git workflow

- Inspect the working tree and preserve existing work. Keep diffs focused on the approved milestone.
- Never rewrite Git history.
- Never commit, push or deploy without approval.
- Use the [pull request template](.github/pull_request_template.md) when opening an approved pull request.

## Evidence collection rules

- Update evidence after every meaningful task.
- Never fabricate evidence or test results.
- Use the [evidence guide](evidence/README.md). Preserve the original prompt, record commands and measured outcomes, and use `Not Recorded` for unavailable session or historical metadata.
- Record only actual capabilities and artifacts. Redact sensitive data and cite only verified screenshot and commit references.

## Definition of done

The approved deliverables are present, the applicable checks have run with outcomes recorded, and meaningful decisions, errors, and limitations are documented. Scope and tenant/security requirements are satisfied where applicable. The milestone record reflects the verified status and any remaining gaps. Git or deployment activity occurs only with approval.
