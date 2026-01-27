---
title: components-column-visibility
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Column Visibility functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.



# components-column-visibility

> Источник: `docs/doc-architect.md` (раздел "CSS-driven Column Visibility через вкладки")

## Архитектура

- `app-ui-root.js` содержит `columnVisibilityConfig`
- `shared/utils/column-visibility-mixin.js` вычисляет `columnVisibilityClasses`
- В `index.html` классы применяются к `<col>`, `<th>`, `<td>`
- CSS: `styles/table-columns.css`

## Принцип

CSS-driven подход: DOM не пересоздается, только меняются классы.

## Bootstrap radio buttons (критическая проблема)

`@change` на `<input class="btn-check">` не срабатывает в Vue из-за скрытия input.

**Решение:** обработчик `@click` на `<label>` вызывает `handleLabelClick` → `handleButtonChange`.
