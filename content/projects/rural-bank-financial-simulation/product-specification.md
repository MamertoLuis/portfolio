---
title: 'Product Specification'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'Source: docs/Product-Specification.md (URB-FSM-PSD-001, v0.2 draft).'
---

Source: `docs/Product-Specification.md` (URB-FSM-PSD-001, v0.2 draft).
Parent: [Rural Bank Financial Simulation](/projects/rural-bank-financial-simulation/rural-bank-financial-simulation/)

Defines product scope, architecture direction, data requirements, model
modules, user workflows, persistence design, controls, and acceptance
criteria. It is a design baseline and does not represent regulatory approval
of the current workbook or model.

## Vision and objectives

Provide a transparent, repeatable, explainable decision-support tool for
assessing how lending, deposit, credit-quality, operating-cost, capital, and
liquidity assumptions affect the bank's projected financial position.

1. Produce integrated balance sheet, income statement, and supporting schedules.
2. Compare a base case with optimistic, adverse, and user-defined stress scenarios.
3. Explain drivers of profitability, asset quality, funding, capital, and liquidity.
4. Validate uploaded data, flag unreconciled amounts, preserve a traceable record.
5. Store runs, assumptions, validation results, and outputs in a relational DB via ORM.
6. Support decisions without replacing the accounting system or professional judgment.

## Users and permissions

| Role | Primary capabilities |
| --- | --- |
| Administrator / Model Owner | Manage model versions, mappings, approved assumptions, access |
| Finance / Accounting | Upload statements, reconcile balances, review projections |
| Lending / Credit / Risk | Supply loan and credit-quality data; review credit assumptions and stress results |
| Treasury / Operations | Supply deposits, funding, cash, liquidity inputs |
| Management / Board | Review dashboards, compare scenarios, export approved reports (read-only) |
| Internal Audit / Compliance | Inspect run history, validation, model versions, change logs (read-only) |

Streamlit UI hiding is not a security boundary; permissions must be enforced
in application services and data access.

## Scope

**Included (initial release)**

- Upload and validate a standardized Excel workbook.
- Convert input worksheets to typed pandas DataFrames; retain a run-specific
  source snapshot/checksum.
- Capture assumptions through Streamlit forms.
- Run integrated projections (initially two years, configurable where approved).
- Calculate performance, loan/deposit movements, credit quality, provisions,
  capital, and liquidity to the extent supported by verified inputs.
- Run named scenarios and compare with the base case.
- Dashboards, data-quality messages, validation results, exportable tables.
- Persist run metadata, assumptions, validation results, and outputs via
  SQLAlchemy ORM.
- Export to Excel and a management-ready report.

**Excluded unless separately approved**

- Core banking replacement, GL posting, loan origination, deposit processing.
- Automated regulatory submission or compliance certification.
- Borrower-level underwriting or automated credit decisions.
- Real-time market-data feeds or external integrations (first release).
- Automatic claims of regulatory compliance.

## User workflow

1. Create a run and select reporting date, horizon, model version, unit scope.
2. Upload the standardized Excel workbook.
3. Review structural, data-quality, and reconciliation results; fix blockers.
4. Configure assumptions via Streamlit forms; select scenarios.
5. Run the simulation (modules execute in dependency order; consistency checks).
6. Review dashboards, comparisons, warnings, key drivers.
7. Save the run (unique ID, timestamp, model version, input reference,
   assumption snapshot, validation report).
8. Export results and supporting schedules.

## Model modules

| Module | Core function | Key outputs |
| --- | --- | --- |
| Input validation | Validate schema, types, periods, duplicates, completeness | Blocking errors, warnings, typed DataFrames |
| Balance sheet | Project asset, liability, equity categories | Projected BS; balance check |
| Income statement | Project interest income/expense, fees, costs, provisions, tax | NII, net income, profitability |
| Loan portfolio | Project balances, releases, collections, write-offs | Gross loans, growth, mix, yield |
| Deposit & funding | Project deposits, flows, funding cost | Closing deposits, mix, cost of funds |
| Credit risk | Estimate delinquency/NPL migration and losses | NPL/past-due, stressed losses |
| Provisions & ROPA | Project allowance, provision expense, ROPA | Provision expense, ACL movement, ROPA |
| Capital | Project equity and eligible capital | Capital ratios and headroom |
| Liquidity | Project cash sources/uses and measures | Liquidity indicators, funding gaps |
| Scenario engine | Apply controlled shocks | Base/optimistic/adverse/stress comparisons |
| Validation | Reconcile statements, check invariants | Exceptions, severity, explanations |
| Reporting | Present dashboards and exports | Charts, tables, run metadata |

## Dependencies and accounting logic

- Loan balances/rates drive interest income; deposit balances/rates drive interest expense.
- Loan performance and credit assumptions drive losses, write-offs, provisions.
- Provisions, interest, and operating costs drive projected profit.
- Profit, capital transactions, distributions drive retained earnings and equity.
- Balance-sheet movements and cash-flow assumptions drive cash and liquidity.
- Total assets must equal total liabilities + equity within documented tolerance.
- Capital/liquidity measures must use verified definitions, not generic proxies
  presented as regulatory results.

## Non-functional requirements

| Area | Requirement |
| --- | --- |
| Usability | Clear upload/configuration flow; actionable errors; consistent terminology |
| Accuracy | Deterministic outputs for identical inputs and model version |
| Traceability | Unique run ID, timestamp, user, input checksum, assumption snapshot, model version, validation report |
| Security | Role-based access; protect confidential data; minimal personal data |
| Data integrity | Validate schemas/types; preserve source values; no silent zeroing |
| Performance | Normal two-year run within 30 seconds |
| Maintainability | Separate UI, calculation modules, mappings, rules, reporting |
| Portability | Run in a bank-approved environment; documented deployment |
| Auditability | Record changes to assumptions, reference data, model versions, actions |
| Export | Excel + management report with scenario, period, units, validation status |

## Acceptance criteria

| ID | Criterion | Expected result |
| --- | --- | --- |
| AC-01 | Valid workbook uploads and maps to schema | Input tables generated |
| AC-02 | Missing sheets/columns, invalid dates, non-numeric amounts detected | Clear error with location and guidance |
| AC-03 | Missing values not silently zeroed | Warning/blocking error by criticality |
| AC-04 | Balance sheet balances within tolerance each period | Pass or explicit difference |
| AC-05 | Income statement reconciles to component schedules | Pass or explicit failure |
| AC-06 | Loan/deposit roll-forwards reconcile | Pass or explicit failure by segment/period |
| AC-07 | Credit-loss/provision calc reproduces approved tests | Pass within tolerance |
| AC-08 | Capital/liquidity outputs use verified definitions | No unverified proxy as regulatory result |
| AC-09 | Identical inputs and version reproduce identical outputs | Regression test passes |
| AC-10 | Scenario changes affect intended assumptions/outputs | Scenario tests pass |
| AC-11 | Run, assumptions, input ref, model version, validation persisted | Traceable run record |
| AC-12 | ORM persistence atomic; respects constraints | Rollback on failure; no partial run |
| AC-13 | Exports agree with on-screen results | Reconciliation test passes |
| AC-14 | Role permissions enforced in service/data layer | Unauthorized actions rejected |

## Delivery roadmap (8 stages)

1. Model specification — formula register, dependency map, data dictionary, rule register.
2. Input template and validator — Excel template, sample workbook, validation report.
3. Core projection engine — balance sheet, P&L, loan and deposit schedules.
4. Credit risk, capital, liquidity — supporting schedules, verified indicators.
5. ORM persistence and run management — entities, migrations, repositories, run history.
6. Streamlit UI and scenario engine — upload, forms, run controls, comparisons.
7. Dashboard and exports — charts, comparisons, Excel/report exports.
8. User acceptance and release — test evidence, user guide, deployment package.

## Definition of initial release success

- Authorized users upload the approved workbook and get actionable validation.
- Core projections reconcile and can be reproduced from a saved run.
- Users change approved assumptions without editing formulas or source data.
- Dashboards communicate base and stress outcomes with definitions and warnings.
- ORM persistence preserves run metadata, assumptions, results, and findings
  with referential integrity.
- No unsupported proxy is presented as verified regulatory compliance.
- Documentation, test evidence, model history, backup/recovery, and user
  instructions exist before release.

## Regulatory alignment

The detailed design must review the current Manual of Regulations for Banks
(MORB), current BSP circulars and reporting instructions, relevant financial
reporting requirements, and the bank's accounting policies. Before presenting a
metric as a regulatory result, document the official rule, effective date,
applicability, formula, reporting basis, required inputs, and test cases. If
not verified, label it an internal planning indicator.
