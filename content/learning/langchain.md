---
title: 'LangChain'
date: '2026-09-21'
draft: false
tags: ['ai', 'langchain', 'tech-stack']
description: 'Experiments with the LangChain framework for chat, tool calling, and memory.'
---

Experiments with the LangChain framework for chat, tool calling, and memory.

- **Version:** 1.4.0
- **Docs:** https://python.langchain.com/docs/
- **API reference:** https://reference.langchain.com/python/

## Notebooks

- `Langchain-ollama-chat.ipynb` — basic chat with a local Ollama model
- `Langgchain-ollama-chat-toolcall.ipynb` — tool/function calling
- `Langchain-persistent-memory.ipynb` — persistent conversation memory
- `langchain-memory-checkpointer.ipynb` — memory checkpointing across sessions

## Gotchas & findings

- LangChain 1.x moved providers into separate packages: `langchain-ollama`, `langchain-deepseek` (no longer `langchain-community`).
- Checkpointer/memory state persists to SQLite at `agent_state.db` (project root).

## Related

- [Project Overview](/learning/project-overview/)
- [Tech Stack](/learning/tech-stack/)
- [Local Models](/learning/local-models/)
