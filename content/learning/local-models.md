---
title: 'Local Models'
date: '2026-09-21'
draft: false
tags: ['ai', 'ollama', 'local-models', 'tech-stack']
description: 'Experiments with locally hosted models via Ollama.'
---

Experiments with locally hosted models via Ollama.

- **Model:** `qwen2.5:3b` (env var `OLLAMA_MODEL`)
- **Docs:** https://ollama.com/ · https://github.com/ollama/ollama

## Notebooks

- `Langchain-ollama-chat.ipynb` — chat via LangChain
- `Langgchain-ollama-chat-toolcall.ipynb` — tool calling via LangChain
- `local-handwriting-recoognition.ipynb` — local handwriting recognition (uses `local_htr` env)

## Gotchas & findings

- Ollama server must be running for notebooks to connect (`ollama serve`).
- Handwriting recognition runs in the separate `local_htr` env (PyTorch + Transformers), not `ai-lab`.

## Related

- [Project Overview](/learning/project-overview/)
- [Tech Stack](/learning/tech-stack/)
- [LangChain](/learning/langchain/)
