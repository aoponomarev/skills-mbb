---
id: process-skill-watcher
title: Skill Watcher & LLM Integration Protocol
scope: skills-mbb
tags: [#process, #automation, #llm, #git]
priority: medium
created_at: 2026-01-31
updated_at: 2026-01-31
---

# Skill Watcher & LLM Integration Protocol

> **Интеллектуальная генерация объяснений для skill-кандидатов через Continue CLI (Mistral/Ollama)**

## Scope

Этот навык описывает архитектуру и принципы работы инструмента `skill-watcher.js`, который автоматически анализирует Git-историю, выявляет потенциальные навыки и генерирует для них объяснения с помощью LLM.

## When to Use

- При отладке работы `skill-watcher.js` (например, если кандидаты не появляются).
- При изменении промптов для генерации объяснений.
- При настройке интеграции с Continue CLI.

## Overview

До внедрения LLM-интеграции `skill-watcher.js` генерировал универсальные шаблонные объяснения, которые были бесполезны. Теперь система использует LLM для создания контекстных описаний архитектурных паттернов.

### Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     skill-watcher.js                            │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Git Log Analysis  →  isSkillWorthy()  →  LLM Call       │  │
│  │        ↓                                      ↓            │  │
│  │  git show <hash>  →  Full Diff (3500 chars) →  Prompt    │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                ↓
┌─────────────────────────────────────────────────────────────────┐
│         Continue CLI Wrapper (localhost:3002/prompt)            │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Mistral 7B (primary)  ↔  Ollama Fallback (backup)       │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                ↓
┌─────────────────────────────────────────────────────────────────┐
│              SKILL_CANDIDATES.json (updated)                    │
│  {                                                               │
│    "explanation": "**Архитектурный паттерн:** ...",            │
│    "source": "Continue Agent (LLM-enhanced)",                  │
│    "regenerated_at": "2026-01-28T12:56:47.679Z"                │
│  }                                                               │
└─────────────────────────────────────────────────────────────────┘
```

## Key Components

1. **`generateExplanationLLM(commitHash, title, files)`**
   - Извлекает полный diff коммита через `git show`
   - Отправляет промпт в Continue CLI API
   - Обрабатывает ошибки и использует fallback при сбое

2. **Prompt Engineering**
   - Identify core architectural pattern
   - Explain WHY valuable for future AI agents
   - Focus on reusable knowledge
   - Write 2-4 sentences in Russian

3. **Operating Modes**
   - **Normal**: `node scripts/skill-watcher.js --days=7` (New commits)
   - **Regeneration**: `node scripts/skill-watcher.js --regenerate` (Update existing)

## Configuration

- **API URL**: `CONTINUE_API_URL=http://localhost:3002`
- **Diff Limit**: 3500 characters (context window optimization)
- **Timeout**: 60s per call

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| **Syntax error: EOF** | Quotes in prompt | Use `**bold**` instead of `<b>` |
| **Diff too large** | >3500 chars | Auto-fallback to template |
| **API Error** | Docker down | `docker-compose up -d continue-cli` |

## Related Skills

- [Autonomous Skill Synthesis](./autonomous-skill-synthesis.md)
- [Continue CLI Integration](../integrations/integrations-continue-cli-mistral.md)
