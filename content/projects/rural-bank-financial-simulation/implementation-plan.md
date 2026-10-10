---
title: 'Implementation Plan'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'Source: docs/Implementation-Plan.md (URB-FSM-IPD-001, v0.1 draft).'
---

Source: `docs/Implementation-Plan.md` (URB-FSM-IPD-001, v0.1 draft).
Parent: [Rural Bank Financial Simulation](/projects/rural-bank-financial-simulation/rural-bank-financial-simulation/) · Related: [Product Specification](/projects/rural-bank-financial-simulation/product-specification/) · [Technical Specification](/projects/rural-bank-financial-simulation/technical-specification/)

Execution plan for building the Financial Simulation Model described in the
Product and Technical Specifications. Defines workstreams, phases, tasks,
dependencies, exit criteria, acceptance-criteria traceability, and risks.

## Guiding principles

1. **Engine first, UI later** — build/test the engine headless before wiring Streamlit.
2. **No formula in the UI** — all numbers come from the engine or persisted results.
3. **Proxies are labeled** — planning proxies are visibly marked; never presented as regulatory results.
4. **Traceability by construction** — every run stores model version, input checksum, assumption snapshot, validation report.
5. **No silent zeros** — missing values are flagged, never zeroed.
6. **Bank sign-off gates regulatory logic** — metrics stay `UNVERIFIED` until verified rules are recorded.

**Definition of Done (per item):** matches specs; unit tests pass and relevant
acceptance tests pass; lint and type checks pass; no secrets/customer data;
docs/docstrings reflect behavior; reviewer confirms exit criterion.

## Workstreams

| WS | Focus |
| --- | --- |
| WS-1 Foundations | Repo scaffold, config, CI, test harness, import boundaries |
| WS-2 Data & Input | Excel reader, template spec, mapping, structural/data-quality validation |
| WS-3 Engine | Balance sheet, P&L, loan/deposit schedules, reconciliation, determinism |
| WS-4 Risk/Capital/Liquidity | Credit risk, provisions/ROPA, capital, liquidity, stress |
| WS-5 Persistence | ORM entities, migrations, repositories, transactions, audit, run management |
| WS-6 UI | Streamlit pages, forms, run controls |
| WS-7 Reporting | Dashboards, Excel/report exports |
| WS-8 Governance | Regulatory rule register, proxy labeling, assumption governance |
| WS-9 QA & Release | Acceptance/regression tests, UAT, deployment package, documentation |

## Phases, tasks, and exit criteria

Phases map to the Product Specification's 8-stage roadmap. Exit criteria are
keyed to acceptance criteria (AC-01…AC-14).

### Phase 0 — Foundations (WS-1, WS-8)
- **F-01** Package layout · **F-02** Tooling (pinning, lint, type, pytest) ·
  **F-03** Settings/config + logging · **F-04** Import-boundary test ·
  **F-05** CI · **F-06** Governance skeleton (rule + proxy registers).
- Exit: repo builds, lint/type/tests green on an empty suite; registers exist and are documented.

### Phase 1 — Model Specification (WS-8, WS-3)
- **M-01** Formula register · **M-02** Module dependency map/execution order ·
  **M-03** Assumption register/ranges · **M-04** Validation rules/tolerances ·
  **M-05** Data dictionary + Excel template schema · **M-06** Register all proxies `UNVERIFIED`.
- Exit: registers reviewed by management/SMEs; every proxy registered; no metric silently labeled regulatory.

### Phase 2 — Input Template and Validator (WS-2)
- **I-01** `template_spec.py` · **I-02** `excel_reader` · **I-03** Structural validation ·
  **I-04** Data-quality validation · **I-05** Account-mapping validation ·
  **I-06** Validation report + persistence · **I-07** Sample workbook + synthetic dataset.
- Exit (AC-01/02/03): valid uploads map; bad structure/date/number gives located errors; missing values flagged not zeroed.

### Phase 3 — Core Projection Engine (WS-3)
- **E-01** `money.py` (Decimal/rounding) · **E-02** Balance sheet + check ·
  **E-03** Loan roll-forward · **E-04** Deposit & funding · **E-05** Income statement ·
  **E-06** Reconciliation · **E-07** Engine entrypoint.
- Exit (AC-04/05/06/09): balance sheet balances; IS reconciles; roll-forwards reconcile; identical inputs reproduce outputs.

### Phase 4 — Credit Risk, Capital, Liquidity (WS-4)
- **R-01** Credit risk · **R-02** Provisions & ROPA · **R-03** Capital ·
  **R-04** Liquidity · **R-05** Stress · **R-06** Unit tests vs approved cases.
- Exit (AC-07/08): credit-loss reproduces approved cases within tolerance; no unverified proxy presented as regulatory result.

### Phase 5 — ORM Persistence and Run Management (WS-5)
- **P-01** Entities/enums · **P-02** Initial + seed migrations · **P-03** Repositories ·
  **P-04** Atomic run save · **P-05** Audit events · **P-06** Reproducibility snapshot ·
  **P-07** Integration tests (migrations/constraints/rollback).
- Exit (AC-11/12): run traceability persists with referential integrity; persistence atomic with no partial run.

### Phase 6 — Streamlit UI and Scenario Engine (WS-6, WS-3)
- **U-01** Router/pages · **U-02** Upload + validation display · **U-03** Grouped assumption forms ·
  **U-04** Run controls · **U-05** RBAC-aware guards · **S-01** Scenario engine ·
  **S-02** Base-vs-scenario comparison · **S-03** Integration/security tests.
- Exit (AC-10/14): scenarios affect intended outputs; RBAC enforced in service/data layer.

### Phase 7 — Dashboard and Exports (WS-7)
- **D-01** Dashboard views with definition display · **D-02** Excel export ·
  **D-03** Management report · **D-04** Proxy badges/warnings · **D-05** Export reconciliation test.
- Exit (AC-13): exports agree with on-screen results; outputs include scenario, period, units, validation status.

### Phase 8 — User Acceptance and Release (WS-9, WS-8)
- **A-01** Run full acceptance suite · **A-02** UAT · **A-03** User guide +
  model/run documentation · **A-04** Backup/recovery + deployment package ·
  **A-05** Bank sign-offs · **A-06** Model version history + release notes.
- Exit: authorized business owner signs off; all acceptance criteria pass or are dispositioned.

## Milestones and dependency graph

```
Phase 0 Foundations
   v
Phase 1 Model Specification
   v
Phase 2 Input Template + Validator
   v
Phase 3 Core Projection Engine ----------+
   v                                    v
Phase 4 Risk/Capital/Liquidity     Phase 5 Persistence
   +---------------+--------------------+
                   v
        Phase 6 UI + Scenario Engine
                   v
        Phase 7 Dashboard + Exports
                   v
        Phase 8 UAT + Release
```

| Milestone | Signal |
| --- | --- |
| M0 Repo ready | CI green, boundaries enforced |
| M1 Design frozen | Registers reviewed; proxies registered |
| M2 Inputs validated | AC-01/02/03 pass |
| M3 Engine reconciles | AC-04/05/06/09 pass |
| M4 Risk/indicators | AC-07/08 pass |
| M5 Persistence atomic | AC-11/12 pass |
| M6 UI + scenarios | AC-10/14 pass |
| M7 Outputs reconcile | AC-13 pass |
| M8 Release | UAT sign-off; all ACs dispositioned |

## Acceptance criteria traceability

| AC | Phase | Verification |
| --- | --- | --- |
| AC-01 valid upload maps | Phase 2 | Integration |
| AC-02 bad structure/date/number | Phase 2 | Unit |
| AC-03 missing not zeroed | Phase 2 | Unit |
| AC-04 balance sheet balances | Phase 3 | Reconciliation |
| AC-05 IS reconciles | Phase 3 | Reconciliation |
| AC-06 roll-forwards reconcile | Phase 3 | Reconciliation |
| AC-07 credit-loss reproduces | Phase 4 | Unit |
| AC-08 verified defs for capital/liquidity | Phase 4 | Governance |
| AC-09 reproducible outputs | Phase 3 | Regression |
| AC-10 scenarios affect outputs | Phase 6 | Integration |
| AC-11 run traceability saved | Phase 5 | Integration |
| AC-12 atomic persistence | Phase 5 | Integration |
| AC-13 exports agree | Phase 7 | Reconciliation |
| AC-14 RBAC enforced | Phase 6 | Security |

## Risk register

| Risk | Impact | Likelihood | Mitigation |
| --- | --- | --- | --- |
| Unreliable/absent historical data | High | High | Inventory data early; synthetic test data; flag uncalibrated base |
| Workbook proxies mistaken for regulatory | High | Medium | Proxy badges; register; AC-08 governance test |
| Inconsistent account mappings | High | Medium | Approve mapping dictionary before Phase 2 exit |
| Credit-risk method indefensible/uncalibrated | High | Medium | Select method; back-test in Phase 1/4 |
| Regulatory rules change/misapplied | Medium | Medium | Versioned rule register; verify effective dates |
| SQLite in multi-user production | High | Low | Prototype only; require approved RDBMS; test concurrency |
| Scope creep to full 3-statement/ECL | High | Medium | Hold initial scope; log changes |
| Determinism/rounding defects | Medium | Medium | Decimal policy; golden regression tests |
| Security boundary gaps | High | Low | Enforce RBAC in service + data layer; security tests |
| Confidential data exposure | High | Low | Minimal PII; masking; no secrets in logs/outputs |

## Open decisions and required sign-offs

| Decision | Gates | Owner |
| --- | --- | --- |
| Data availability | Phase 2–4 | Management / data owners |
| Account mapping dictionary | Phase 2 | Finance |
| Credit-risk method | Phase 4 | Risk / Credit committee |
| Regulatory definitions verified | Phase 4, 8 | Compliance |
| Historical period & horizon | Phase 1 | Business owner |
| Unit-level scope | Phase 1 | Business owner |
| Assumption ownership | Phase 1 | Management |
| Production DB choice | Phase 5, 8 | IT / InfoSec |
| Deployment & security environment | Phase 8 | IT / InfoSec |
| Retention & backup policy | Phase 8 | IT / Compliance |

## Estimation basis and assumptions

- Effort expressed in phases/tasks; sizing refined in Phase 1 once Appendix B artifacts are approved.
- Phases 4 and 5 may run partly in parallel after Phase 3.
- Assumes initial two-year horizon and portfolio-level detail (not borrower-level).
- Working assumptions: bank-approved Python runtime; synthetic/masked data;
  SME reviewers available; workbook is a reference not a spec of record;
  production DB/deployment approved before Phase 8.
- Change control: scope changes logged and assessed; no regulatory formula
  implemented without a verified rule-register entry.
