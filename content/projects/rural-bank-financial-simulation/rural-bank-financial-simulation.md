---
title: 'Rural Bank Financial Simulation'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'A Python-based financial projection and stress-testing application for'
---

A Python-based financial projection and stress-testing application for
**Upland Rural Bank of Dalaguete, Cebu Inc.** (Head Office: Oslob;
Branch-Lite Unit: Dalaguete). Authorized users upload standardized financial
and portfolio data, configure forecast assumptions through a Streamlit
interface, run integrated projections and stress scenarios, review
reconciliations and warnings, and export results.

- **Project folder (repo):** `~/Workspace/urbd-financial-simulation`
- **Planning horizon:** two-year projection (configurable horizon preferred)
- **Institution:** Upland Rural Bank of Dalaguete, Cebu Inc.
- **Currency:** Philippine peso (PHP) unless a source field states otherwise

## Purpose

Provide a transparent, repeatable, and explainable decision-support tool that
helps management assess how lending, deposit, credit-quality, operating-cost,
capital, and liquidity assumptions may affect the bank's projected financial
position. It does not replace the accounting system, regulatory returns, the
credit approval process, or professional judgment.

## Stack

| Layer | Choice |
| --- | --- |
| Frontend | Streamlit |
| Application / calculation | Python (3.11+) |
| Data processing | pandas |
| Excel input/output | openpyxl |
| Charts | Plotly |
| ORM | SQLAlchemy 2.x |
| Migrations | Alembic |
| Validation | Pydantic v2 |
| Database (dev) | SQLite |
| Database (prod) | PostgreSQL or bank-approved RDBMS |
| Testing | pytest |

## At a glance

| Item | Value |
| --- | --- |
| Current stage | Phase 0 (Foundations) complete; engine and UI not yet built |
| Documents | 3 specs (Product, Technical, Implementation) + reference workbook |
| Acceptance criteria | AC-01 … AC-14 |
| Regulatory posture | Planning proxies only; no output is a verified regulatory result |

## Notes in this project

- [Product Specification](/projects/rural-bank-financial-simulation/product-specification/) — product scope, users, modules, NFRs, acceptance criteria
- [Technical Specification](/projects/rural-bank-financial-simulation/technical-specification/) — architecture, data model, engine, formula register
- [Implementation Plan](/projects/rural-bank-financial-simulation/implementation-plan/) — workstreams, phases 0–8, risks, open decisions
- [Reference Model (Workbook)](/projects/rural-bank-financial-simulation/reference-model-workbook/) — the 15-sheet Excel model and proxy register
- [Build Status](/projects/rural-bank-financial-simulation/build-status/) — what exists now and what comes next

## Regulatory notice

The current reference workbook is a **management planning model**, not a
verified regulatory calculation. It uses several proxies (zero 2026 base
inputs, a flat 4% cost of funds, RWA = assets × density, and planning CAR/MLR).
These must be replaced with actual figures and BSP-verified formulas before any
output is presented as a regulatory compliance result. No BSP circular number,
threshold, or deadline is asserted without verification against the official
source.
