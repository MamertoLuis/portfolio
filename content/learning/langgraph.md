---
title: 'LangGraph'
date: '2026-09-21T00:00:00+08:00'
draft: false
tags: ['ai', 'langgraph', 'tech-stack']
description: 'Experiments with LangGraph for stateful, graph-based agent workflows.'
---

Experiments with LangGraph for stateful, graph-based agent workflows.

- **Version:** 1.2.11
- **Docs:** https://langchain-ai.github.io/langgraph/

## Notebooks

- `Langgraph-RAG.ipynb` — retrieval-augmented generation (uses `faiss-cpu` vector store)
- `Langchain-HITL.ipynb` — human-in-the-loop workflows (filename says "Langchain")

## Gotchas & findings

- Checkpointer persists graph state to `agent_state.db` (SQLite).
- RAG uses `faiss-cpu` for the vector store.

## Related

- [Project Overview](/learning/project-overview/)
- [Tech Stack](/learning/tech-stack/)
- [LangChain](/learning/langchain/)
