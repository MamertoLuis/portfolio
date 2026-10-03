---
title: 'Environments'
date: '2026-09-21T00:00:00+08:00'
draft: false
tags: ['ai', 'conda', 'tech-stack']
description: 'Conda environments on this machine (Anaconda distribution via miniconda3).'
---

Conda environments on this machine (Anaconda distribution via miniconda3).

| Env | Python | Purpose | Key packages |
|-----|--------|---------|--------------|
| `ai-lab` | 3.11.16 | Main notebook experiments | langchain 1.4.0, langgraph 1.2.11, jupyter, ipykernel 7.3.0 |
| `local_htr` | 3.10 | Handwriting recognition (CUDA) | pytorch 2.5.1 (cu121), torchvision 0.20.1, transformers 5.17.0, opencv-python 5.0.0 |
| `pyside-env` | — | GUI apps | pyside6 6.11.2 |

## Activate

```bash
conda activate ai-lab        # main
conda activate local_htr     # handwriting recognition
conda activate pyside-env    # GUI
```

## Reproduce

```bash
conda env create -f environment.yml   # ai-lab
```

Pinned exact versions: `conda-lock.txt`.

## Related

- [Tech Stack](/learning/tech-stack/)
- [Skills](/learning/skills/)
- [Project Overview](/learning/project-overview/)
