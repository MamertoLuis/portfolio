---
title: 'Prompting Basics'
date: '2026-09-21'
draft: false
tags: ['ai', 'prompting', 'foundations']
description: 'Core prompting techniques for steering LLM output without fine-tuning. Follows on from the LLM fundamentals concepts (tokens, context window, temperature, sampling).'
---

Core prompting techniques for steering LLM output without fine-tuning. Follows on from the LLM fundamentals concepts (tokens, context window, temperature, sampling).

## Key techniques

| Technique | What it does | When to use |
|-----------|--------------|-------------|
| **System / role prompt** | Sets persona, tone, and constraints for the whole conversation | Consistent voice, hard rules, output format |
| **Zero-shot** | Instructions only | Simple, well-known tasks |
| **Few-shot** | A few input→output examples in the message list | Labeling, format mimicry, edge-case steering |
| **Delimiters** | Tags (``<review>``, triple backticks, ``###``) separate instructions from data | Long or untrusted input; avoids prompt injection |
| **Explicit schema** | State exact output fields/values | Structured output (JSON), parsing downstream |
| **Templates** | Fixed instructions + variable slots | Reuse across calls; `string.Template`, LangChain `PromptTemplate` |

## Notebook

- `prompting-basics.ipynb` — runnable examples for system prompts, zero/few-shot, delimiters, temperature sweep, and templates.
  - Provider toggle: `deepseek` (default) or `ollama` (`ollama serve` required).
  - Keys loaded via `load_dotenv()` — never hardcoded.

## Gotchas & findings

- Put durable rules in the **system** message; put the task in the **user** message.
- Few-shot examples cost tokens every call — keep them short and representative.
- Always delimit raw user input so instructions and data cannot be confused.
- `temperature=0` is best for classification/extraction; raise it only for creative tasks.
- Templates prevent drift between experiments and production prompts.

## Related

- Learning Path
- [Cloud APIs](/learning/cloud-apis/)
- [Local Models](/learning/local-models/)
- [Project Overview](/learning/project-overview/)
