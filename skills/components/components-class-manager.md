---
title: components-class-manager
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Class Manager functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-class-manager

> Источник: `docs/doc-comp-classes.md`

## Универсальный механизм classesAdd/classesRemove

Все компоненты поддерживают `classesAdd` и `classesRemove` (Object). Ключи соответствуют внутренним элементам (`root`, `icon`, `label`, `suffix`, `menu`, `button`).

## Обработка классов

Использовать `window.classManager.processClassesToString(base, add, remove)`:
1. Базовые классы
2. Удаление `classesRemove`
3. Добавление `classesAdd`

## Важные правила

- Никаких жестко заданных классов в шаблонах (кроме базовых Bootstrap).
- Не использовать старый `:class` для дочерних компонентов — только `:classes-add` / `:classes-remove`.
- Объекты классов в computed должны иметь фиксированную структуру (все ключи присутствуют).

## Селекторы с instanceHash

Для `querySelector` экранировать спецсимволы или использовать `data-instance-hash`.
