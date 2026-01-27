---
title: components-localization
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Localization functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-localization

> Источник: `docs/doc-comp-principles.md` (раздел "Система централизованной реактивности текстов")

## Базовый принцип

Все текстовые элементы UI используют централизованную реактивную локализацию (RU/EN) без перезагрузки страницы.

## Ключевые элементы

- `uiState.tooltips.currentLanguage` — реактивное состояние языка
- `core/config/tooltips-config.js` — единый источник текстов
- `tooltipsConfig.getTooltip(key)` — доступ к текстам

## Обязательные правила

- `currentLanguage` только в `computed`, не в `data()`
- В `computed` с текстом обязательно `const lang = this.currentLanguage;`
- Нельзя использовать `TOOLTIPS_RU/TOOLTIPS_EN` напрямую
- Нельзя кэшировать тексты в `data()`
- Всегда добавлять ключи одновременно в RU и EN
