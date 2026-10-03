---
title: 'AGENTS.md'
date: '2026-09-21'
draft: false
tags: ['ai', 'notebooks', 'project']
description: 'AGENTS.md is the instruction file that guides AI coding agents working in the project folder:'
---

> Notes on the agent-guidance file for the AI notebook experiments project.

## What it is

`AGENTS.md` is the instruction file that guides AI coding agents working in the project folder:

- **Path:** `/home/marty-manguerra/Workspace/ai/AGENTS.md`
- **Purpose:** turn the folder from a "human reminder" into machine-readable guidance for agents.

## Key sections

- **Environment** — Anaconda Python; activate with `conda activate ai-lab`
- **Secrets** — API keys live in `.env`, loaded via `python-dotenv`; never hardcode keys in notebooks. `credentials.json` is a sensitive Google OAuth config.
- **Environment variables** — `OLLAMA_MODEL`, `DEEPSEEK_REASONING_MODEL`, `DEEPSEEK_CODING_MODEL`, `DEEPSEEK_API_KEY`, `GEMINI_MODEL`, `GEMINI_API_KEY`
- **Experiment areas** — LangChain (chat, tool calling, memory), LangGraph (RAG, HITL), Ollama (local), DeepSeek + Gemini (cloud), document AI (PDF form extraction, handwriting recognition)
- **Notebook conventions** — one experiment per notebook, `<framework>-<topic>.ipynb` naming, clear sensitive outputs before saving

## Repo hygiene

- Git repo initialized with branch `main`
- `.gitignore` protects `.env`, `credentials.json`, `*.db`, `.ipynb_checkpoints/`, `.vscode/`

## Related

- Welcome
