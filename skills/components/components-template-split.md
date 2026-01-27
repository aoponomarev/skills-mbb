---
title: components-template-split
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Template Split functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-template-split

> Источник: `docs/doc-architect.md` (раздел "Вынос x-template шаблонов")

## Принцип

Все `x-template` вынесены в отдельные файлы и вставляются в DOM при загрузке модуля.

## Структура

- `shared/templates/{component-name}-template.js` — переиспользуемые компоненты
- `app/templates/{component-name}-template.js` — компоненты приложения
- `features/<feature>/templates/` — зарезервировано

## Формат

Каждый файл содержит константу `TEMPLATE` со строкой шаблона и регистрацию `<script type="text/x-template">` в DOM.

## Порядок загрузки

Шаблоны должны быть загружены **до** Vue и компонентов. Загрузка через `core/module-loader.js`.
