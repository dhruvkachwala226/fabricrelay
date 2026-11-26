# FabricRelay

FabricRelay is a proposed multi-tenant B2B textile catalog and wholesale inquiry management platform. The first use case is a Surat textile wholesaler sharing a branded catalog with buyers who need to identify a fabric, review its specs and colorways, and request a physical sample or wholesale quote. It is a solo project for the Build with Codex One-Month Challenge.

## Proposed workflows

Wholesalers will manage fabric photos, design codes, GSM, width, colorway availability, MOQ and optional indicative rates in one branded catalog per business. They can share a stable catalog link through WhatsApp or another channel. Buyers will browse and filter published fabrics, then request a sample or quote without signing in. Wholesalers will manage incoming requests in their own inquiry queue and follow up through existing channels.

| Role | Planned access |
| --- | --- |
| `SUPER_ADMIN` | Manage wholesaler accounts and business profiles, activate or suspend businesses, and review recent platform activity |
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

M00 established the repository documentation and evidence pack. M01 documents the problem hypotheses, proposed MVP and validation protocol. Customer validation and product development remain pending.

- [Milestone plan](PLANS.md)
- [M01 discovery documents](evidence/01-understand.md#m01--understand-and-problem-validation)
- [Engineering and agent instructions](AGENTS.md)
- [Challenge evidence](evidence/README.md)
