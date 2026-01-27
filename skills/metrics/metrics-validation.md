---
title: metrics-validation
tags:
  - "#mbb-spec"
  - "#metrics"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Metrics Validation functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# metrics-validation

> Источник: `docs/doc-architect.md` (раздел "Валидация и схемы данных")

## Схемы и валидация

- Схемы: `core/validation/schemas.js`
- Валидация: `core/validation/validator.js`
- Нормализация: `core/validation/normalizer.js`
- Проверка диапазонов: `core/validation/math-validation.js`

## Назначение

Единая проверка входных данных и расчетов для предотвращения ошибок в моделях и метриках.
