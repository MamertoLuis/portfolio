---
title: 'Tech Stack'
date: '2026-09-21'
draft: false
tags: ['ai', 'tech-stack']
description: 'Versions captured from environment.yml / conda-lock.txt (ai-lab env) on 2026-09-21.'
---

Versions captured from `environment.yml` / `conda-lock.txt` (ai-lab env) on 2026-09-21.

## Frameworks & libraries

| Tech | Version | Purpose | Docs | Notes |
|------|---------|---------|------|-------|
| LangChain | 1.4.0 | orchestration | [docs](https://python.langchain.com/docs/) | [LangChain](/learning/langchain/) |
| LangGraph | 1.2.11 | stateful agents | [docs](https://langchain-ai.github.io/langgraph/) | [LangGraph](/learning/langgraph/) |
| langchain-ollama | 1.1.0 | Ollama integration | [docs](https://python.langchain.com/docs/integrations/chat/ollama/) | [Local Models](/learning/local-models/) |
| langchain-deepseek | 1.1.0 | DeepSeek integration | [docs](https://python.langchain.com/docs/integrations/chat/deepseek/) | [Cloud APIs](/learning/cloud-apis/) |
| google-genai | 1.65.0 | Gemini SDK | [docs](https://ai.google.dev/gemini-api/docs) | [Cloud APIs](/learning/cloud-apis/) |
| openai | 2.26.0 | OpenAI-compatible SDK | [docs](https://platform.openai.com/docs) | — |
| faiss-cpu | 1.15.0 | vector store (RAG) | [docs](https://github.com/facebookresearch/faiss) | [LangGraph](/learning/langgraph/) |
| python-dotenv | 1.2.2 | load `.env` secrets | [docs](https://saurabh-kumar.com/python-dotenv/) | [AGENTS](/learning/agents/) |
| PyTorch | 2.5.1 (CUDA 12.1) | handwriting recognition | [docs](https://pytorch.org/docs/) | [Environments](/learning/environments/) |
| Transformers | 5.17.0 | HTR models | [docs](https://huggingface.co/docs/transformers/) | [Document AI](/learning/document-ai/) |
| PySide6 | 6.11.2 | GUI | [docs](https://doc.qt.io/qtforpython-6/) | [Environments](/learning/environments/) |

## Models

| Provider | Model | Env var |
|----------|-------|---------|
| Ollama (local) | qwen2.5:3b | `OLLAMA_MODEL` |
| DeepSeek | deepseek-reasoner | `DEEPSEEK_REASONING_MODEL` |
| DeepSeek | deepseek-chat | `DEEPSEEK_CODING_MODEL` |
| Gemini | gemini-3.1-flash-lite | `GEMINI_MODEL` |

## Related

- [Environments](/learning/environments/)
- [Project Overview](/learning/project-overview/)
- [Skills](/learning/skills/)
