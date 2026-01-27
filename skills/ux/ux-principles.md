---
title: ux-principles
tags:
  - "#mbb-spec"
  - "#ux"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Ux Principles functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# ux-principles

> Источник: `docs/doc-comp-principles.md` (раздел "Единый источник правды для заголовков модальных окон")

## Единый заголовок для меню и модалки

**Требование:** Заголовок модального окна должен совпадать с текстом пункта меню/кнопки, которая его открывает.

**Обоснование:** Консистентность UX и отсутствие путаницы. Единый источник правды через `modals-config.js` синхронизирует заголовки во всех местах.

## Реализация

- Все заголовки берутся из `core/config/modals-config.js`
- Использовать `modalsConfig.getModalTitle(modalId)` в модальном окне и в меню
