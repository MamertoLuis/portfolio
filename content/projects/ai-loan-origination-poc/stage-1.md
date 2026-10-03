---
title: 'Stage 1'
date: '2026-10-04T00:00:00+08:00'
draft: false
description: 'A loan-application form processing pipeline. PDFs dropped into in/ are'
---

A loan-application form processing pipeline. PDFs dropped into `in/` are

extracted with the Gemini vision model, validated deterministically, persisted

to SQLite, and reported on in Markdown. Ships with a CLI and a Flask web UI.

  

## Overview

  

Given a scanned/digital loan application form (PDF), loanapp will:

  

1. Convert PDF pages to images and extract structured key/value fields using

Gemini (`gemini-2.5-flash` by default) via LangGraph.

2. Normalise free-form model keys against a canonical field catalogue

(`backend/config.py`) and validate the result: missing values, mandatory

fields (with conditional rules, e.g. spouse when married), inconsistencies

(age vs. date of birth, civil status vs. spouse...), and math checks

(totals, ages, terms).

3. Write a Markdown validation report to `reports/`, persist the submission

to `submissions.db` (SQLite), and move the PDF from `in/` to `out/`.

  

Extraction results are cached in `tmp/<sha256>.json`, so re-processing the

same file never spends API tokens again.

  

## Architecture

  

```

app.py / run.py entrypoints (CLI, dev server)

backend/

config.py paths, Gemini settings, model pricing, field catalogue

schema.py Pydantic models for the form (schema v2)

pipeline.py LangGraph graph: prepare -> cache? -> extract -> validate

-> persist -> finalize

validators.py deterministic checks + Markdown report rendering

persistence.py SQLite submissions table, typed reads/writes

cli.py batch run, --list, --show

web/

app.py Flask UI: upload, submission list/detail, JSON API

templates/, static/

tests/ pytest suite (schema, validators, persistence, web, nodes)

in/ out/ tmp/ reports/ working directories

```

  

Pipeline state flows through nodes in `backend/pipeline.py`:

`prepare_node` hashes the file, `cache_router` short-circuits on a cache hit,

`pdf_to_image_node` renders pages with pdf2image (poppler required),

`google_vision_extraction_node` calls Gemini, `validation_node` runs the

validator and renders the report, `persist_node` writes SQLite + report, and

`finalize_node` moves the PDF to `out/`.

  

## Setup

  

```bash

python -m venv .venv && source .venv/bin/activate

pip install -r requirements.txt

# poppler-utils must be installed for pdf2image

# create .env and set GEMINI_API_KEY

```

  

Environment variables (see `backend/config.py`):

  

| Variable | Default | Purpose |

| --- | --- | --- |

| `GEMINI_API_KEY` | — | required for fresh extraction |

| `GEMINI_MODEL` | `gemini-2.5-flash` | vision model |

| `PDF_DPI` | `200` | PDF render resolution |

| `MAX_UPLOAD_MB` | `25` | web upload size cap |

| `SUBMISSIONS_DB` | `./submissions.db` | SQLite path |

| `WEB_HOST` / `WEB_PORT` / `FLASK_DEBUG` | `127.0.0.1` / `5000` / `1` | dev server |

  

## Usage

  

CLI (batch process everything in `in/`):

  

```bash

python app.py # process in/*.pdf

python app.py --list # list persisted submissions

python app.py --show <submission_id>

```

  

Web UI / JSON API:

  

```bash

python run.py # http://127.0.0.1:5000

```

  

Routes: `GET /` (dashboard + upload), `POST /upload`,

`GET /submissions/<id>`, `GET /api/submissions`, `GET /api/submissions/<id>`.

  

## Testing

  

```bash

pytest

```

  

## Development history (summary)

  

- **2026-09-17** — first working form extraction prototype artifacts

(`sample_loan_application.pdf`, `data.json`).

- **2026-09-17 → 2026-10-03** — OCR experiments: PaddleOCR prototype in the

sibling `ocr-test/` project, superseded by a Gemini vision extraction script;

first validation report + token/cost tracking (`reports/*.md`, model

pricing table).

- **2026-10-04** — refactor into the current structure: `backend/` package

(config/schema/validators/persistence/pipeline/cli), Flask web UI, SQLite

persistence, pytest suite, and a second end-to-end run of the pipeline.
