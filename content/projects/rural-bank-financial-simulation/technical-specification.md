---
title: 'Technical Specification'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'Source: docs/Technical-Specification.md (URB-FSM-TSD-001, v0.1 draft).'
---

Source: `docs/Technical-Specification.md` (URB-FSM-TSD-001, v0.1 draft).
Parent: [Rural Bank Financial Simulation](/projects/rural-bank-financial-simulation/rural-bank-financial-simulation/) · Related: [Product Specification](/projects/rural-bank-financial-simulation/product-specification/)

Translates the Product Specification into an implementable technical design:
architecture, stack, module layout, data model, Excel input schema,
calculation engine, validation, persistence, security, and test strategy.
This is a design baseline — it contains no source code and does not certify the
reference workbook.

## Architecture

Layered, modular, with a framework-agnostic calculation engine.

```
User
  v
Streamlit UI  (upload, forms, run controls, Plotly dashboards, exports; NO formulas)
  v
Application / service layer  (RunService, InputService, AssumptionService,
       ScenarioService, ReportingService, AuthService; transactions; RBAC)
  +---------------------------+
  v                           v
Domain calculation engine     Persistence (SQLAlchemy ORM + repositories)
  - pure functions/modules      - ORM entities, unit of work
  - pandas in/out               - Alembic migrations
  - Decimal money
  - no DB/UI/I/O
  v                           v
Typed result objects          Relational database
+ validation report           (SQLite dev; PostgreSQL/approved prod)
```

**Binding rules**

1. Streamlit modules contain no financial formulas.
2. The engine runs headless: `engine.run(snapshot, assumptions) -> RunResult`.
3. All DB access flows through repositories invoked by services.
4. pandas is the in-memory processing format only; not the system of record.
5. Every persisted run links to model version, assumption snapshot, input
   checksum/reference, and validation report.
6. Saving a run is atomic (one transaction); no partial runs.
7. Secrets/credentials are loaded from environment/secret store, never committed.

## Technology stack

| Concern | Choice | Notes |
| --- | --- | --- |
| Language | Python 3.11+ | Match bank-approved runtime |
| UI | Streamlit | Presentation only |
| Data | pandas | Tabular IO/calculations |
| Excel | openpyxl | Read input, write export |
| Charts | Plotly | Interactive dashboards |
| ORM | SQLAlchemy 2.x | Declarative typed models |
| Migrations | Alembic | Schema versioning |
| Validation | Pydantic v2 | Config, forms, schemas, DTOs |
| DB (dev) | SQLite | Single-user only |
| DB (prod) | PostgreSQL / approved | Bank-approved RDBMS |
| Testing | pytest | Unit/integration/regression |
| Config | pydantic-settings | Env-based configuration |
| Logging | stdlib logging | Structured, no secrets |

**Numeric policy:** money is `decimal.Decimal`, persisted as `NUMERIC` with
explicit precision/scale (default `18,2`; ratios `10,6`). No `float64` for
persisted monetary values. Rounding applied at defined boundaries only
(round-half-up for PHP, ratios to 6 decimals).

**Date/time policy:** business dates are timezone-naive `date`; audit
timestamps timezone-aware UTC.

## Module / package layout

```
urbd-financial-simulation/
  app.py                      # Streamlit entrypoint (thin router)
  pyproject.toml
  alembic.ini                 # (Phase 5)
  migrations/                 # (Phase 5)
  src/fsm/
    config/                   # settings, constants, enums, registry/
    schemas/                  # Pydantic DTOs and IO schemas
    io/                       # excel_reader, excel_writer, template_spec
    engine/                   # context, modules/, registry, run, money
    validation/               # structural, data_quality, reconciliation
    services/                 # run, input, assumption, scenario, reporting, auth
    orm/                      # base, entities/, enums
    repositories/             # run, assumption, result, audit, reference
    reporting/                # dashboards, exports
    ui/                       # pages/, components/
    security/                 # rbac
    tests/
```

**Layer rule:** `ui -> services -> {engine, repositories} -> orm`. The engine
must not import services, repositories, orm, or ui (enforced by an
import-boundary test).

## Data model (ORM)

SQLAlchemy 2.x declarative, typed `Mapped[...]`. Integer surrogate PKs;
monetary `Numeric`; controlled values as enums; `created_at`/`updated_at` on
mutable entities.

| Entity | Purpose | Key fields |
| --- | --- | --- |
| `users` | Identity and role | id, username (UQ), display_name, role, is_active, timestamps |
| `model_versions` | Released calculation logic | id, version (UQ), description, status, created_at, approved_at/by |
| `simulation_runs` | Main run record | id, run_name, reporting_date, forecast_start/end, unit_scope, status, model_version_id, created_by, timestamps |
| `input_files` | Uploaded workbook tracking | id, run_id, original_filename, sha256, storage_key, template_version, uploaded_at/by |
| `assumption_sets` | Scenario/config header | id, run_id, scenario_name, description, approved_status, created_at |
| `assumption_values` | Individual assumptions | id, assumption_set_id, key, value_numeric/text, unit, effective_period, source_note |
| `input_validation_results` | Structural/data-quality issues | id, run_id, sheet_name, row_ref, column_name, severity, code, message, resolved_at |
| `simulation_results` | Run-level/period measures | id, run_id, scenario_name, period, metric_code, metric_value, unit, dimension_key |
| `financial_statement_lines` | Normalized statement outputs | id, run_id, statement_type, period, account_code, amount, unit_scope, source_type |
| `scenario_definitions` | Reusable scenario templates | id, name (UQ), description, status, created_by, timestamps |
| `scenario_shocks` | Scenario overrides | id, scenario_id, assumption_key, shock_type, shock_value, effective_period |
| `audit_events` | Trace key actions | id, user_id, event_type, entity_type, entity_id, event_time, details_json |

**Enums:** Role; ModelVersionStatus (DRAFT/APPROVED/RETIRED); RunStatus;
UnitScope (BANK/HO/BLU); ApprovalStatus; Severity
(INFO/WARNING/ERROR/BLOCKING); StatementType; SourceType
(ACTUAL/FORECAST/DERIVED); ScenarioStatus; ShockType (ABSOLUTE/RELATIVE/MULTIPLIER).

**Migration plan:** Alembic owns all schema changes; initial revision creates
all tables/enums/FKs/indexes; enum additions are additive; reference seed data
via data migrations.

## Excel input template schema

Template version declared on `Control` and validated. ISO-8601 dates; amounts
declare unit and sign convention; duplicate keys rejected/resolved; missing
values flagged, never zeroed; uploaded file retained as checksummed reference.

Sheets: `Control`, `Balance_Sheet`, `Income_Statement`, `Loan_Portfolio`,
`Deposits`, `Credit_Risk`, `Capital`, `Liquidity` (conditional),
`Historical_Data` (optional), `Reference_Data`.

The full column dictionary is maintained declaratively in `template_spec.py`.
The reference workbook is a management projection model, not the standardized
input template; the template is a superset that records underlying balances so
the engine computes ratios rather than using single proxy fields.

## Calculation engine

Pure, deterministic functions; money in Decimal; each module returns outputs
plus diagnostics. Execution order:

```
1  Input validation & mapping
2  Reference data / account mapping
3  Balance sheet projection (assets, liabilities, equity)
4  Loan portfolio roll-forward (open -> releases -> collections -> write-offs -> close)
5  Deposit & funding
6  Income statement
7  Credit risk (migration, NPL, stressed losses)
8  Provisions & ROPA
9  Capital (equity, eligible capital, ratios)
10 Liquidity (sources/uses, gap, indicators)
11 Balance-sheet / statement reconciliation
12 Scenario engine (apply shocks, re-run affected)
13 Reporting assembly (metrics + statement lines)
```

**Invariants:** total assets = liabilities + equity within tolerance (default
PHP 0.01); income statement reconciles to schedules; loan/deposit roll-forwards
reconcile by segment/period; capital roll-forward reconciles.

**Formula register (excerpt)** — `[P]` = planning proxy:

| Reference item / formula | Engine mapping | Proxy |
| --- | --- | --- |
| Total assets = prior × (1 + asset growth) | bs.total_assets | — |
| Gross loans = prior × (1 + loan growth) | loan.closing | — |
| Gross NPL = gross loans × NPL ratio | credit.npl | [P] |
| Interest income = avg gross loans × NIM | pl.interest_income | [P] |
| Interest expense = avg deposits × 4.0% | pl.interest_exp | [P] |
| Provision expense = avg loans × provision rate | pl.provision | [P] |
| Net income = pretax − tax | pl.net_income | — |
| RWA = total assets × RWA density | capital.rwa | [P] |
| Planning CAR = capital / RWA proxy | capital.car | [P] |
| Planning MLR = eligible liquid / qualifying liab | liq.mlr | [P] |
| Closing ACL = opening + provision − write-offs + recoveries | provisions.movement | [P] |
| Stress CAR = stressed capital / RWA | stress.car | [P] |

Proxy metrics expose an `is_proxy=True` annotation and the UI renders a
"planning proxy / not a regulatory result" notice wherever they appear.

## Validation and reconciliation

**Categories:** structural (sheets/columns/version); data quality (types,
dates, duplicates, missing values, classifications); reconciliation
(accounting invariants and cross-schedule agreement).

**Severity:** INFO (note) · WARNING (proceed with caution) · ERROR (blocks
affected module) · BLOCKING (blocks entire run).

**Tolerances:** balance check `|assets − (liab + equity)| <= PHP 0.01`;
statement-to-schedule and roll-forward reconciliations exact within rounding
policy. Missing values are classified (not-provided / not-applicable / lost),
never silently zeroed.

## Persistence and transactions

A run save is one transaction across `simulation_runs`, `input_files`,
`assumption_sets`/`assumption_values`, `input_validation_results`,
`simulation_results`, `financial_statement_lines`, and `audit_events`; failure
rolls back with no partial run. The workbook is stored under a controlled path
and recorded by `sha256` + `storage_key` (no large blobs by default).
Reproducibility snapshot: run id, timestamp, user, input checksum, assumption
snapshot, model version, validation report.

## Security and RBAC

Roles: ADMIN, FINANCE, CREDIT_RISK, TREASURY, MANAGEMENT, AUDIT. Permissions
are enforced in `AuthService` and repository guards, not only by hiding UI
widgets. MANAGEMENT and AUDIT are read-only for approved runs. Minimal personal
data; no secrets/passwords/credentials in logs.

## Regulatory rule register

Before a metric is a regulatory result, record: rule_id, source,
effective_date, applicability, formula, reporting_basis, required_inputs,
status (UNVERIFIED/VERIFIED/RETIRED), and test_cases. All proxy metrics in the
formula register begin as `UNVERIFIED`. Candidate sources to verify: BSP MORB,
BSP circulars and reporting instructions, PFRS 9 / BSP ECL guidance. No
circular number, threshold, or deadline is asserted until verified.

## Open technical decisions

- Forecast horizon configuration (business owner)
- Unit-level projection scope (HO/BLU) (business owner)
- Account mapping dictionary (finance)
- Credit-risk method: ECL/PFRS 9 staging vs proxy (risk/credit)
- Production DB (IT/InfoSec)
- Deployment topology, auth mechanism (IT/InfoSec)
- Retention policy (IT/compliance)
