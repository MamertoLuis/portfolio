---
title: 'Project Skills'
date: '2026-09-21T00:00:00+08:00'
draft: false
tags: ['ai', 'skills', 'project']
description: 'Custom agent skills for this project, stored in .pi/skills/.'
---

Custom agent skills for this project, stored in `.pi/skills/`.

## notebook-workflow

- **Path:** `.pi/skills/notebook-workflow/SKILL.md`
- **When it loads:** creating, editing, running, or reviewing `.ipynb` notebooks
- **Covers:** `conda activate ai-lab`, kernel setup, loading secrets from `.env`, naming/output conventions

## anaconda-workflow

- **Path:** `.pi/skills/anaconda-workflow/SKILL.md`
- **When it loads:** activating, creating, listing, or installing into conda envs; kernel troubleshooting
- **Covers:** env inspection, package install (conda-first, pip fallback), exporting `environment.yml`, Jupyter kernel registration

## Notes

- Skills are discovered at startup, so new skills require a pi restart to auto-load.
- Force-load with `/skill:notebook-workflow` or `/skill:anaconda-workflow`.
- Project skills only load once the project is trusted.

## Related

- [Project Overview](/learning/project-overview/)
- [AGENTS](/learning/agents/)
