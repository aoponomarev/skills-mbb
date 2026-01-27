---
title: components-modal-buttons
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Modal Buttons functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-modal-buttons

> Источник: `docs/doc-comp-principles.md` (раздел "Система управления кнопками модального окна")

## Базовый принцип

`cmp-modal` предоставляет `modalApi` через `provide/inject`. Кнопки регистрируются один раз и могут отображаться в header и footer одновременно.

## API

- `registerButton(buttonId, config)`
- `updateButton(buttonId, updates)`
- `removeButton(buttonId)`
- `getButton(buttonId)`
- `getButtonsForLocation(location)`

## Обязательные правила

- "Отмена" — обязательно в footer, слева (`classesAdd.root = 'me-auto'`)
- "Сохранить" — в footer, два состояния: `primary` → `success`
- Кнопки удалять в `beforeUnmount()`
- Проверять наличие `modalApi` перед использованием

## Ссылки

- `shared/components/modal.js`
- `shared/components/modal-buttons.js`
