---
title: components-icon-manager
tags:
  - "#mbb-spec"
  - "#components"
  - "#icons"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---
## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-icon-manager

## Scope
- Единый источник URL иконок монет с приоритетом CDN и fallback.

## Key Components
- `core/api/icon-manager.js`

## Key Rules
- Приоритет у собственного CDN, внешний источник используется только как fallback.
- Конфликты имен решаются через alias map.
- Cache busting зависит от версии приложения.

## Workflow
1) Добавить/обновить иконку в CDN.
2) Обновить alias map при необходимости.
3) Проверить корректные URL и fallback.

## References
- `core/api/icon-manager.js`
