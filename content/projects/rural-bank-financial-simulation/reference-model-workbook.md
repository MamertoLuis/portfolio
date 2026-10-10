---
title: 'Reference Model (Workbook)'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'File: docs/UplandRuralBankIntegratedFinancialProjection2027-2028.xlsx'
---

File: `docs/Upland_Rural_Bank_Integrated_Financial_Projection_2027-2028.xlsx`
Parent: [Rural Bank Financial Simulation](/projects/rural-bank-financial-simulation/rural-bank-financial-simulation/) · Related: [Technical Specification](/projects/rural-bank-financial-simulation/technical-specification/)

The reference model is a **15-sheet, fully linked Excel workbook** for a
two-year management projection (2027–2028). It is a **management planning
model**, not a verified regulatory calculation, and it is a reference — not the
standardized input template.

## Model convention

The workbook's 2026 base inputs are **all zero** and must be replaced with
actual 2026 financial statement figures (the source material was BSP circulars,
not the bank's statements). Yellow cells are editable assumptions/placeholders;
blue cells are calculated or regulatory references.

## Sheets

| Sheet | Contents |
| --- | --- |
| `Dashboard` | Key projection outputs and prudential indicators (2027, 2028, change) |
| `Cover` | Purpose, model conventions, regulatory anchors, sources, limitations |
| `Inputs` | 2026 base balance sheet + 2027/2028 assumptions + regulatory parameters |
| `Projection` | Integrated balance sheet with balance-check row |
| `P&L` | Income statement projection (interest, fees, opex, provisions, tax, ROA/ROE) |
| `Asset Quality` | NPL/PDL/ROPA ratios, ACL coverage |
| `Capital & Liquidity` | Planning CAR/MLR and headroom, reserve requirement |
| `Regulatory Checks` | Pass/fail checks vs minimum CAR/MLR/capitalization and targets |
| `Loan & Credit` | Loan portfolio, growth, NPL/PDL, provision, ACL coverage |
| `Deposit & Funding` | Deposit balances/growth, implied funding cost, liquidity ratio |
| `Provisions & ROPA` | ACL movement schedule and ROPA schedule (movement inputs are placeholders) |
| `Stress Testing` | Illustrative sensitivity: NPL uplift, extra provisions, capital/CAR and liquid-asset haircut |
| `Ratios & Benchmarks` | ROA/ROE, NIM, cost-to-income, asset-quality and capital ratios; peer values are placeholders |
| `Scenario Analysis` | Editable growth/NIM/provision/opex adjustments and approximate net-income bridge |
| `Model Validation` | Reconciliation checks (balance sheet, sums, net income, capital roll-forward, thresholds) |

## Core linked chain

```
Inputs  ->  Projection (balance sheet + check)  ->  P&L
                    |                                   |
                    v                                   v
             Asset Quality  ------>  Capital & Liquidity  ------>  Regulatory Checks
                    ^                                   ^
                    |                                   |
             Loan & Credit                        Deposit & Funding
                    |                                   |
                    +---------- Provisions & ROPA -------+
                    +---------- Stress Testing ----------+
                    +---------- Ratios & Benchmarks -----+
                    +---------- Scenario Analysis -------+
                    +---------- Model Validation --------+
```

## Key formulas (as implemented)

- Total assets = prior × (1 + asset growth); gross loans = prior × (1 + loan growth).
- Gross NPL = gross loans × NPL ratio; gross PDL = gross loans × PDL ratio.
- ROPA = total assets × ROPA/assets ratio.
- ACL = average gross loans × provision rate.
- Cash & eligible liquid assets = total assets × liquid/assets ratio.
- Deposits = prior × (1 + deposit growth).
- Capital = prior + net income × (1 − payout).
- Interest income = average gross loans × NIM; **interest expense = average deposits × 4.0%**.
- Non-interest income = average assets × ratio; opex = average assets × ratio.
- Provision expense = average gross loans × provision rate.
- Net income = pretax − tax; ROA = NI / avg assets; ROE = NI / avg equity.
- RWA (planning proxy) = total assets × RWA density; CAR = capital / RWA.

## Proxy register (must be replaced before regulatory use)

| ID | Proxy | Location | Replacement requirement |
| --- | --- | --- | --- |
| PX-001 | 2026 base inputs = 0 (placeholders) | Inputs | Populate with actual 2026 figures |
| PX-002 | Cost of funds = fixed 4% | P&L | Derive from deposit product rates and funding mix |
| PX-003 | RWA = total assets × density | Capital & Liquidity | Compute from verified risk weights/exposure classes |
| PX-004 | Planning CAR / MLR on proxy RWA and liabilities | Capital & Liquidity; Dashboard | Replace with verified BSP CAR/MLR formulas and inputs |
| PX-005 | ACL = average gross loans × provision rate | Projection; Provisions & ROPA | Use approved provisioning method (PFRS 9 / applicable framework) |
| PX-006 | ROPA = total assets × ratio | Projection | Model from actual ROPA schedules and movements |
| PX-007 | Peer benchmarks illustrative | Ratios & Benchmarks | Replace with sourced peer statistics |

## Regulatory anchors recorded in the workbook (reference only)

The workbook notes a rural/cooperative-bank MLR prudential minimum of 20%, a
risk-based CAR minimum of 10% for stand-alone rural/cooperative banks, a
current reserve-requirement reference of 0% for RB/Coop deposits (from
28-Mar-2025), and a rural-bank minimum capitalization of PHP 50 million
(head-office-only and up to 5 branches; branch-lite units excluded from the
branch count for this rule). **These are reference values, not a verified rule
register** — confirm against the current MORB / BSP issuances before board
adoption.

## Model limitations (per the workbook)

Yellow cells are editable planning assumptions/placeholders. RWA, MLR,
provisioning, and peer benchmarks are management proxies and must be reconciled
to official bank reports and approved policy before use. This is a management
planning tool, not the official BSP CAR/MLR report.
