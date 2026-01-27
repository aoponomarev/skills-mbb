---
title: components-bootstrap
tags:
  - "#mbb-spec"
  - "#components"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Components Bootstrap functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# components-bootstrap

> Источник: `docs/doc-comp-principles.md` (раздел "Стратегия максимальной совместимости с Bootstrap")

## Обязательные требования совместимости

1. Инициализация через Bootstrap JS API (`new bootstrap.Dropdown()`, `new bootstrap.Modal()`).
2. Подписка на события Bootstrap (`show.bs.dropdown`, `hide.bs.dropdown`, `shown.bs.modal`).
3. Программный доступ к API (`show()`, `hide()`, `toggle()`, `getBootstrapInstance()`).
4. Уничтожение экземпляров в `beforeUnmount()` через `dispose()`.
5. Сохранение нативных data‑атрибутов (`data-bs-*`).
6. Использование только Bootstrap классов/переменных для тем.

## Запрет кастомных стилей

Кастомный CSS, inline‑стили и `<style>`‑блоки запрещены, кроме минимальных исключений. Основа — Bootstrap классы и утилиты.
