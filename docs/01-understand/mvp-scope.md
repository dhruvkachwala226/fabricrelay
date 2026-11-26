# One-month MVP scope

The release centers on one loop. An approved wholesaler publishes a fabric, shares a link, and receives a sample or wholesale quote request with the right fabric and color context.

| Priority | Scope |
| --- | --- |
| Must Have | Two authenticated roles and guest buyers. Super Admin provisions and activates or suspends businesses and sees minimal activity. Wholesaler branding, one catalog per business, fabric drafts with photos and specs, named colorways with availability, MOQ and optional indicative rate. Stable shareable URL, guest catalog browsing and basic search and filters. Distinct sample and quote forms and a tenant inquiry queue with basic status. Security, isolation, mobile, accessibility, performance, reliability, maintainability and privacy acceptance criteria apply. Covers FR-001–FR-011 and NFR-001–NFR-008. |
| Should Have | Inquiry type and status filters, and optional filters for width and GSM (FR-012 and FR-013). Prioritize only after the complete Must Have loop passes verification. |
| Could Have | CSV import, catalog templates, internal inquiry notifications and bilingual labels, subject to pilot evidence and time. |
| Out of Scope | Payment gateways, checkout, subscription billing, full ERP, logistics management, native mobile apps, AI-generated fabric photographs, real-time chat, automated WhatsApp integration, a multi-seller marketplace and customer account management. |

## Product boundaries

- A copied link works in existing communication channels. One catalog per business and manual onboarding keep initial setup manageable.
- Availability is a manually maintained colorway status, with no stock reservation or exact inventory ledger. Rates may be indicative or withheld from public view. Staff finalize terms outside the product.
- A sample request captures interest, and a quote request captures intended quantity. The wholesaler arranges delivery, payment and follow-up externally.
- Start with the complete published-fabric and inquiry path using synthetic data, then exercise tenant isolation and suspension. Treat the [requirements](requirements.md) as release checks and the [success metrics](success-metrics.md) as pilot measures.
