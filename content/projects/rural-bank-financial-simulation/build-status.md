---
title: 'Build Status'
date: '2026-10-10T00:00:00+08:00'
draft: false
description: 'Parent: Rural Bank Financial Simulation · Related: Implementation Plan'
---

Parent: [Rural Bank Financial Simulation](/projects/rural-bank-financial-simulation/rural-bank-financial-simulation/) · Related: [Implementation Plan](/projects/rural-bank-financial-simulation/implementation-plan/)

Current state of the repository at `~/Workspace/urbd-financial-simulation`.
Stage: **Phase 0 (Foundations) complete, minus CI** (as agreed).

## Environment

- Python **3.12.14** virtual environment at `.venv` (created with `uv`).
- App + dev dependencies installed via `uv pip install -e ".[dev]"`.
- Excel MCP server dependencies preserved in the same venv; `excel-mcp-server`
  console script runs.
- Git repository initialized (no remote, **no commits yet**).

## Phase 0 task status

| Task | Status |
| --- | --- |
| F-01 Package layout (TSD §4.1) | Done |
| F-02 Tooling: pinning, lint, type, pytest | Done |
| F-03 Settings + logging (pydantic-settings) | Done |
| F-04 Import-boundary test (TSD §4.2) | Done |
| F-05 CI (lint + type + tests) | **Deferred** |
| F-06 Governance skeleton (rule + proxy registers) | Done |

Exit criteria met: repo builds with lint/type/tests green; registers exist and
are documented.

## What exists

```
pyproject.toml            # PEP 621, hatchling src-layout; ruff + mypy strict + pytest
.gitignore                # .venv/, *.db, .env, node_modules/, caches
.env.example              # FSM_DATABASE_URL, FSM_STORAGE_PATH, FSM_LOG_LEVEL, FSM_ENVIRONMENT
README.md                 # overview + setup/development commands
app.py                    # thin Streamlit entrypoint stub (no formulas)
src/fsm/
  config/  settings.py, logging.py, registry/{loader.py, *.json}
  schemas/ io/ engine/modules/ validation/ services/
  orm/entities/ repositories/ reporting/ ui/pages/ ui/components/
  security/rbac.py        # Role + Permission enums, permission map
  tests/test_import_boundaries.py
```

Governance registers (all entries `UNVERIFIED`):

- `regulatory_rule_register.json` — RR-001…RR-005 (CAR, MLR, minimum
  capitalization, reserve requirement, PFRS 9 ECL).
- `proxy_register.json` — PX-001…PX-007 (must-replace proxies).

## Verification results

| Check | Result |
| --- | --- |
| `ruff check .` | Clean |
| `mypy src` | Clean (23 source files) |
| `pytest` | 5 passed, 18 skipped (skips = non-boundary layers) |
| `import fsm` | OK (`fsm.__version__ == "0.1.0"`) |
| Settings + registers load | OK |
| Excel MCP server | `excel-mcp-server --help` OK |

## Deferred / not yet built

- **F-05 CI** — no workflow yet; no git remote configured.
- **Git MCP server** — requested then deferred; also, the project
  `opencode.json` uses a non-standard MCP shape (to revisit).
- **Alembic `alembic.ini` + `migrations/`** — scheduled for Phase 5.
- **Engine modules, Excel IO, services, ORM entities, Streamlit pages** —
  later phases.

## Next steps

1. Decide on CI host and add the Phase 0 F-05 workflow (lint + type + tests).
2. Begin **Phase 1 — Model Specification**: finalize the formula register,
   module dependency map, assumption register, validation rules, and Excel
   template schema; keep all proxies `UNVERIFIED`.
3. Or begin **Phase 2 — Input Template and Validator** if the design artifacts
   are already acceptable.
4. Resolve open decisions (data availability, account mapping, credit-risk
   method, production DB) via the owners listed in [Implementation Plan](/projects/rural-bank-financial-simulation/implementation-plan/).
