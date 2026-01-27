---
title: libs-metadata-generation
tags:
  - "#mbb-spec"
  - "#libs"
  - "#metadata"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---
## When to Use

- При необходимости работы с данным компонентом или функционалом.



# libs-metadata-generation

## Scope
- Генерация и загрузка метаданных монет (например, `coins.json`).

## Key Components
- `core/api/coins-metadata-generator.js`
- `core/api/coins-metadata-loader.js`

## Key Rules
- Источник метаданных централизован, UI не должен хранить собственные копии.
- Обновление метаданных выполняется через генератор, загрузка — через loader.

## Workflow
1) Генерировать метаданные и публиковать файл.
2) Загружать метаданные через loader в приложение.
3) Проверять совместимость структуры.

## References
- `core/api/coins-metadata-generator.js`
- `core/api/coins-metadata-loader.js`
