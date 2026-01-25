---
title: components-layout-alignment
tags: [#mbb-spec, #components, #layout]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# components-layout-alignment

## Scope
- Выравнивание высоты элементов через vertical padding.
- Spacing и выравнивание компонентов в ряду.

## When to Use
- Когда элементы одного размера должны иметь одинаковую высоту.
- При выравнивании компонент в горизонтальном ряду.

## Key Rules
- Вертикальный padding задается **внутреннему контейнеру** по классу размера.
- Горизонтальный padding остается Bootstrap (`px-*`).
- Spacing — через Bootstrap утилиты.

## Workflow
1) Убрать фиксированный `py-*` из внутреннего контейнера.
2) Привязать padding к размеру через селектор `.component-responsive.size-class > .inner-container`.
3) Значения брать из нативных размеров Bootstrap (см. CSS‑комменты).

## References
- `styles/wrappers/button.css`
- `shared/components/button.js`
