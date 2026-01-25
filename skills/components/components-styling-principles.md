---
title: components-styling-principles
tags:
  - "#mbb-spec"
  - "#components"
  - "#ui"
  - "#css"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# components-styling-principles

## Scope
- Принципы стилизации компонентов.
- Нейтральные тона для светлой/темной темы.
- Документирование CSS.

## When to Use
- Когда добавляете или правите стили компонентов.
- При ревью UI на консистентность.

## Key Rules
- **Bootstrap first**: приоритет утилитам и классам Bootstrap.
- **Нейтральные тона**: `text-secondary` + opacity, без хардкода HEX/RGB.
- **Фоны**: `bg-body`/`bg-transparent` + `border-secondary` при необходимости.
- **CSS‑детали в коде**: конкретику (размеры/цвета/отступы) держать в комментариях CSS.

## Workflow
1) Применить утилиты Bootstrap и проверить тему.
2) Для нейтральных состояний использовать `secondary` + opacity.
3) Конкретные значения документировать в `styles/`.

## References
- `docs/doc-comp-principles.md`
- `docs/doc-architect.md`
