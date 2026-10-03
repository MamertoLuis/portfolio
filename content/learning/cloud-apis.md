---
title: 'Cloud APIs'
date: '2026-09-21T00:00:00+08:00'
draft: false
tags: ['ai', 'cloud-apis', 'tech-stack']
description: 'Experiments with hosted model APIs.'
---

Experiments with hosted model APIs.

- **DeepSeek:** `DEEPSEEK_API_KEY`, `DEEPSEEK_REASONING_MODEL` (deepseek-reasoner), `DEEPSEEK_CODING_MODEL` (deepseek-chat) · docs: https://api-docs.deepseek.com/
- **Gemini:** `GEMINI_API_KEY`, `GEMINI_MODEL` (gemini-3.1-flash-lite) · docs: https://ai.google.dev/gemini-api/docs

## Notebooks

- `deepseek-form-extract.ipynb` — form extraction with DeepSeek
- `gemini-form-extract.ipynb` — form extraction with Gemini
- `prompting-basics.ipynb` — prompting techniques via OpenAI-compatible endpoints (DeepSeek default, Ollama optional). See [Prompting Basics](/learning/prompting-basics/)

## Gotchas & findings

- `deepseek-reasoner` (reasoning) vs `deepseek-chat` (general/coding) are distinct models.
- Gemini uses the `google-genai` SDK; DeepSeek uses `langchain-deepseek` / OpenAI-compatible API.

## Related

- [Project Overview](/learning/project-overview/)
- [Tech Stack](/learning/tech-stack/)
- [Document AI](/learning/document-ai/)
- [Prompting Basics](/learning/prompting-basics/)
