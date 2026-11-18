# J$A / MINT//DOCS ERPNext integration foundation

Status: Planning only. No production migration or ERPNext version upgrade has occurred.

## Architecture decision
- Preserve the existing MINT//DOCS interface as the primary staff GUI.
- ERPNext/Frappe provides the authoritative business backend; ERPNext Desk remains available for advanced administration.
- Build MINT-specific functionality as a separate Frappe custom app rather than patching ERPNext core wherever possible.
- VERTORIQ//AI acts through authenticated, permission-scoped APIs with approval gates and audit trails.
- J$A Command can aggregate permitted venture metrics, but must not mix finances, identities or customer records across ventures.
- TREK'T and other ventures integrate via APIs where useful, not by merging their application code into ERPNext.

## Source evidence
| Baseline | ERPNext | Python | Frappe |
|---|---|---|---|
| GitHub develop | 16.0.0-dev | >=3.10 | >=16.0.0-dev,<17.0.0 |
| User-uploaded erpnext-develop.zip | 17.0.0-dev | >=3.14 | >=17.0.0-dev,<18.0.0 |

The ZIP is an unverified development snapshot, not a verified release. These version declarations do not establish compatibility, completeness or security.

## Implementation gates
1. Identify and inspect the actual MINT//DOCS frontend codebase, deployment and existing APIs.
2. Compare v16 and v17 trees, custom changes, schema migrations, dependencies and supported deployment environment.
3. Define MINT domain model: lead, customer, approved catalogue item, pro-forma, order, BOM, supplier purchase, manufacturing job, QC, delivery, final invoice and payment reconciliation.
4. Preserve MINT business rules: separate J/P numbering, only pro-formas and final invoices, 0% VAT as currently approved, 80% deposit, verified cleared funds, price-list floors, approval and audit controls.
5. Implement read-only API adapters first, then explicit permissioned writes with idempotency.
6. Prototype a standalone Frappe app and test with synthetic data. Never commit secrets or customer records.
7. Build and test a disposable Frappe 17 environment, including migration and regression checks.
8. Review results in a draft PR; require explicit approval before merge or deployment.

## Immediate tasks
- [ ] Locate MINT//DOCS source and hosting
- [ ] Produce detailed v16/v17 compatibility report
- [ ] Design custom Frappe app structure and API contracts
- [ ] Define tenancy/permission boundaries for each venture
- [ ] Establish CI tests and staging environment
- [ ] Prepare upgrade PR after successful validation

## Non-goals
- No wholesale overwrite of ERPNext v16 with v17.
- No replacement of the existing MINT//DOCS GUI.
- No production database migration, customer communication or financial action without approval.
