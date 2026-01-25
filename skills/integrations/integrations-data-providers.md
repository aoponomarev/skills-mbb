---
title: integrations-data-providers
tags:
  - "#mbb-spec"
  - "#integrations"
  - "#data-providers"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---

# integrations-data-providers

## Scope
- Единый интерфейс провайдеров данных о монетах и рынокe.
- Конфигурация провайдеров и переключение источников.

## Key Components
- `core/api/data-provider-manager.js`
- `core/api/data-providers/base-provider.js`
- `core/api/data-providers/coingecko-provider.js`
- `core/api/coingecko-stablecoins-loader.js`
- `core/config/data-providers-config.js`

## Key Rules
- Все провайдеры реализуют общий интерфейс `BaseProvider`.
- Переключение источника не требует изменения потребителей данных.
- Конфигурация провайдеров централизована, без дублирования в компонентах.

## Workflow
1) Добавить/обновить провайдера в `core/api/data-providers/`.
2) Подключить в `data-provider-manager`.
3) Описать параметры в `data-providers-config`.

## References
- `core/api/data-provider-manager.js`
- `core/api/data-providers/base-provider.js`
- `core/api/data-providers/coingecko-provider.js`
- `core/api/coingecko-stablecoins-loader.js`
- `core/config/data-providers-config.js`
