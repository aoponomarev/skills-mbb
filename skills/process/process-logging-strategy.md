---
id: process-logging-strategy
title: MBB Logging & Tracking Strategy
scope: skills-mbb
tags: [process, logging, tracking, maintenance]
priority: medium
created: 2026-01-31
updated: 2026-01-31
---

# MBB Logging & Tracking Strategy

## Purpose
Обеспечить прозрачность работы системы, фиксацию истории изменений и эффективный дебаггинг через разделение логов на функциональные категории.

## Log Categories & Locations

Все логи и трекеры находятся в директории `logs/`.

### 1. Tracking Logs (SSOT History)
Эти файлы отслеживаются Git и являются частью базы знаний. Используются для планирования и отчетности.

| File | Purpose | When to Update |
| :--- | :--- | :--- |
| `changelog.md` | История версий и крупных изменений. | При выпуске новой версии или завершении этапа. |
| `fixes-tracking.md` | Список всех исправленных багов и примененных фиксов. | После каждого успешного исправления ошибки. |
| `issues-backlog.md` | Бэклог известных проблем, багов и техдолга. | При обнаружении новой проблемы. |

### 2. Operational Logs (Agent Session Context)
Файлы для передачи контекста между сессиями ИИ-агентов.

| File | Purpose | When to Use |
| :--- | :--- | :--- |
| `session-report.md` | Отчет о проделанной работе в текущей сессии. | В конце каждой сессии. |
| `handoff-note.md` | Инструкции для следующего агента. | Перед завершением работы, если задача не доделана. |

### 3. System Runtime Logs (Ignored by Git)
Технические логи, генерируемые скриптами в реальном времени. Помогают в дебаггинге.

| File | Content | Origin Tool |
| :--- | :--- | :--- |
| `skills-events.log` | Единый лог всех событий SMB автоматизации. | `server.js`, `skill-processor.js` |
| `infra-manager.log` | Лог управления контейнерами и автовосстановления. | `infra-manager.js` |
| `mcp-debug.log` | Отладочная информация MCP сервера. | `skills-mcp/server.js` |
| `alert-status.json` | Текущий статус алертов (cooldowns, active alerts). | `alert-manager.js` |

## When to Consult Logs (AI Agent Triggers)

1.  **Start of Session**: Проверь `handoff-note.md` и `issues-backlog.md`, чтобы понять текущий приоритет.
2.  **After an Error**:
    - Проверь `logs/skills-events.log` (последние 50 строк).
    - Если ошибка в MCP, проверь `logs/mcp-debug.log`.
3.  **After a Success**: Запиши детали в `fixes-tracking.md` (если это был фикс) или `changelog.md`.
4.  **End of Session**: Сгенерируй `session-report.md` и обнови `handoff-note.md`.

## Quality Gates
- Имена логов всегда в **lowercase** с дефисами (AI-friendly).
- Трекеры (`.md`) должны иметь заголовок и обратную хронологию.
- Runtime логи (`.log`) никогда не должны попадать в коммиты (проверяй `.gitignore`).

## See Also
- [Infrastructure Maintenance](./process-infrastructure-maintenance.md)
- [Bug Resolution Protocol](./process-bug-resolution-protocol.md)
