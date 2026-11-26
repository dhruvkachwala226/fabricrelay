# FabricRelay

FabricRelay is a proposed multi-tenant B2B textile catalog and wholesale inquiry management platform for textile wholesalers and their buyers. It is a solo project for the Build with Codex One-Month Challenge.

## Proposed workflows

Wholesalers will digitize fabric swatches, maintain textile specifications, colorways, prices and availability, and publish branded catalogs. Buyers will browse published fabrics and request physical samples or wholesale quotations without signing in. Wholesalers will manage incoming requests for their own business.

| Role | Planned access |
| --- | --- |
| `SUPER_ADMIN` | Platform administration through explicitly authorized workflows |
| `WHOLESALER_ADMIN` | Catalog and inquiry administration within an authorized wholesale tenant |
| Public buyer | Published catalog browsing and sample or quotation inquiries without authentication |

## Planned stack

| Area | Technology |
| --- | --- |
| Application and UI | Next.js App Router, TypeScript, Tailwind CSS, shadcn/ui |
| Data and media | MongoDB, Mongoose, Cloudinary |
| Authentication and validation | Auth.js, Zod |
| Verification | Vitest, Playwright |
| CI and hosting | GitHub Actions, Vercel |

## Project status

M00 covers repository documentation and the challenge evidence pack. Application setup, development commands, and deployment are planned for later approved work.

- [Milestone plan](PLANS.md)
- [Engineering and agent instructions](AGENTS.md)
- [Challenge evidence](evidence/README.md)
