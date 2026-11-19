# MINT//DOCS ↔ ERPNext API boundary (draft v0.1)

## Verified discovery
- Existing Wix site: MINT OS Factory & CRM, site ID 2bdd658a-c912-462f-8f29-7d5ff72c8564, published on a free plan, Velo enabled, updated 2026-11-07.
- This Wix site has not been verified as the current MINT//DOCS application; no frontend source or API contract was recovered from this inspection.
- No MINT-named GitHub repository found in owner repository search.
- The ERPNext source repository is not a running ERPNext/Frappe server.

## Interface approach
- MINT//DOCS frontend authenticates to a dedicated backend-for-frontend (BFF) or controlled Frappe endpoint. Never embed ERPNext administrator tokens in client-side code.
- BFF maps authenticated users and scoped venture permissions to ERPNext/Frappe actions.
- Only the ERP backend owns canonical customer, order, inventory, manufacturing and financial transaction records.
- AI receives least-privilege, read-only tools by default. All official issuance, price changes, payment reconciliation, customer communication and financial writes require policy checks and explicit approval.

## Proposed API (not deployed)
| Method | Endpoint | Behaviour | Permission |
|---|---|---|---|
| GET | /api/mint/v1/catalogue | Approved catalogue versions and hard-floor prices | sales:read |
| GET | /api/mint/v1/customers | Scoped customer lookup | crm:read |
| POST | /api/mint/v1/proformas/drafts | Create unnumbered draft, calculate deposit and balance | sales:draft |
| POST | /api/mint/v1/proformas/{draft_id}/issue | Server-assign J/P number atomically after approval | sales:issue |
| GET | /api/mint/v1/orders/{id} | Production, stock and payment state | orders:read |
| POST | /api/mint/v1/orders/{id}/release | Requires reconciled cleared balance and QC approval | orders:release |
| GET | /api/mint/v1/reports/margins | Authorised cost vs selling-price analytics | reports:financial |

## Non-negotiable invariants
1. Salesperson numbering series J and P are independent, continuous, allocated server-side at issuance, never on draft creation.
2. Catalogue base prices are floors. Omitting an included component cannot reduce price; only authorised upgrades add amounts.
3. 80% deposit, balance before release; proof of EFT is not proof of cleared funds.
4. Only pro-formas and final invoices are MINT document types. Final invoice is issued at completion/collection.
5. Approved current VAT treatment is 0%; tax settings must be validated with qualified accounting review before live invoicing.
6. Supplier costs, stock consumption, BOM revisions and production steps are audit logged.
7. Venture-specific permissions, data partitioning and financial records are mandatory; no implicit sharing.
8. All mutation requests use idempotency keys and server-side authorisation; retain user, timestamp, before/after audit evidence.

## Next engineering blockers
- Locate current MINT//DOCS code, deployed URL, hosting environment, and authentication provider.
- Confirm Frappe server deployment and version; ERPNext GitHub source alone does not expose a running backend.
- Design DocTypes, permission matrix, integration tests and staging with synthetic records.
- Verify v17 runtime and migration compatibility before implementing any core upgrade.
