---
title: core-systems-workspace-config
tags:
  - "#mbb-spec"
  - "#core-systems"
  - "#config"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---
## When to Use

- При необходимости работы с данным компонентом или функционалом.



# core-systems-workspace-config

## Scope
- Централизованное хранилище настроек рабочей зоны.

## Key Components
- `core/config/workspace-config.js`

## Key Rules
- Все настройки рабочей зоны определяются в одном месте.
- Компоненты получают настройки через единый интерфейс.

## Workflow
1) Добавить/обновить настройки в `workspace-config`.
2) Обновить потребителей настроек.
3) Проверить значения по умолчанию.

## References
- `core/config/workspace-config.js`
