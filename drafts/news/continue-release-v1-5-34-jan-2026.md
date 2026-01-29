---
id: continue-release-v1-5-34-jan-2026
title: "Continue Release: Shareable Agents & Code Review Inbox (Jan 9, 2026)"
description_ru: "Обновление Continue v1.5.34: Публичные ссылки на агентов для обмена воркфлоу и Inbox для код-ревью (управление PR прямо в IDE)."
scope: "External News: Continue AI Update"
tags: [#news, #continue, #agents, #codereview]
priority: medium
created_at: 2026-01-29
updated_at: 2026-01-29
utility_score: 8
user_comment: "Shareable Agents — отличная штука для того, чтобы делиться нашими кастомными агентами для MBB (например, skill-processor) с другими участниками или для тестирования."
impact_analysis: "Функция 'Shareable Agents' позволит нам экспортировать конфигурации наших специализированных агентов MBB в виде ссылок. Это упрощает миграцию и настройку новых рабочих мест. 

'Code Review Inbox' даст возможность агенту MBB мониторить входящие PR в репозиториях (skills, libs) более структурированно, превращая процесс ревью в управляемый поток задач. Это повышает прозрачность работы автономных систем и человека.

SCORE: 8"
---

# Continue Release: Shareable Agents & Code Review Inbox (Jan 9, 2026)

## Overview
Continue v1.5.34 introduces collaboration features that bridge the gap between individual agents and team workflows.

## Key Features
- **Shareable Agents**: Generate public links to your agent configurations.
- **Code Review Inbox**: Manage Pull Requests, merge conflicts, and failing checks within the Continue interface.
- **Additional Instructions**: Just-in-time prompts for agents without modifying the base config.

## Architectural Utility
For MBB, this means our **Custom Agents** (like the ones defined in `protocol-agent-core`) can be versioned and shared more easily. The Code Review Inbox is a perfect candidate for a new skill that allows the agent to handle MBB PRs autonomously.

## Scoring
- **Implementation Priority**: 8/10
- **Automation Potential**: High (PR management)
- **Collaboration**: High (sharing workflows)
