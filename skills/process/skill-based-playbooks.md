---
id: skill-based-playbooks
title: Skills as Agentic Playbooks
description_ru: Знания как «инструменты в рюкзаке»: подход к обучению ИИ-агентов через независимые файлы-инструкции. Каждый навык становится компактной «кассетой», которую агент может загрузить в свою память для выполнения конкретной роли. Это делает интеллект системы модульным, легким для обновления и не зависящим от огромных баз данных.
scope: Conceptualizing skills as "cassettes" or "playbooks" that provide agents with specific capabilities and autonomy.
tags: [#architecture, #ux, #agentic]
priority: medium
created_at: 2026-01-28
updated_at: 2026-01-29
---

# Skills as Agentic Playbooks

> "Skills as Playbooks (Cassettes/CDs) - The agent works with files, not databases."

## Scope

This skill defines the philosophy of using Markdown-based "Skills" as the primary way to extend agent capabilities.

## When to Use

- When designing new features that require AI intervention.
- When documenting complex procedures for future agents.
- When building "agentic" tools that need to be portable and easy to edit.

## Overview

Instead of hard-coding logic into a database or a complex graph, we use **Markdown Skills**. These are like "playbooks" or "cassettes" that an agent can "insert" into its context to perform a specific task.

## Key Principles

1. **File-Centric**: Skills live as files in the repository. This makes them version-controlled, searchable, and easy for both humans and agents to read.
2. **Portability**: A skill should be self-contained. It includes the "What", "Why", "How", and "Examples".
3. **Agent-Readable**: Use clear headings and structured data (YAML front-matter) so agents can parse metadata quickly.
4. **Hybrid Nature**: A skill is a mix of natural language instructions (prompts) and technical guidelines (code patterns).

## Implementation

- **Storage**: `skills/` or `skills-mbb/` directories.
- **Discovery**: Use `list_skills` or `grep` to find the right playbook for the current task.
- **Execution**: The agent reads the file and follows the `Guidelines` and `Steps`.

## Validation

- New capabilities are added by creating a `.md` file, not by changing core application code.
- Agents can successfully explain a skill's purpose just by reading its `scope` and `overview`.
