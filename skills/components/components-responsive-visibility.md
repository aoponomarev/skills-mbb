---
title: components-responsive-visibility
tags: [#mbb-spec, #components, #responsive]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# components-responsive-visibility

## Scope
- Правила адаптивности компонентов (mobile‑first).
- Классы компонентов и элементов.

## When to Use
- Когда нужно скрывать/показывать текст и иконки по ширине.
- При добавлении новых компонентов с короткими/полными лейблами.

## Key Rules
- Единый брейкпоинт: `576px` (Bootstrap SM).
- Логика видимости реализуется **в CSS**, не в JS.
- Компоненты получают классы состояния (`has-icon`, `has-label-short`).

## Workflow
1) Добавить класс `*-responsive` на компонент.
2) Использовать элементные классы (`.icon`, `.label`, `.label-short`).
3) Реализовать правила через `@media (min-width: 576px)` в `styles/wrappers/`.

## References
- `styles/wrappers/`
- `docs/doc-comp-implementation.md`
