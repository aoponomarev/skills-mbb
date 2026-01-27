---
title: components-tooltips
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Tooltips functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.



# components-tooltips

> Источник: `docs/doc-comp-principles.md` (раздел "Система всплывающих подсказок")

## Принцип

Используются только нативные подсказки через `title`. HTML запрещен.

## Структура подсказки

1. Статическая часть из `tooltips-config.js`
2. Разделитель `\n`
3. Динамическая интерпретация из `tooltip-interpreter.js`

## Использование

- Метрики футера: `window.tooltipInterpreter.getTooltip(key, { value, lang })`
- Контролы хедера: статический текст + `getInterpretation()`
- Простые элементы: только `tooltipsConfig.getTooltip()`

## Ограничения

- Только UTF-8
- Длина до ~2000 символов
