---
title: integrations-rate-limiting
tags:
  - "#mbb-spec"
  - "#integrations"
  - "#rate-limit"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---

# integrations-rate-limiting

## Scope
- Централизованное управление лимитами запросов к внешним API.

## Key Components
- `core/api/rate-limiter.js`

## Key Rules
- Лимиты задаются централизованно, без копирования в отдельных провайдерах.
- Всплески запросов ограничиваются через общий контроллер.

## Workflow
1) Определить лимиты и стратегию (token/bucket/interval).
2) Подключить лимитер к вызовам API.
3) Обновить/проверить общие метрики ошибок 429.

## References
- `core/api/rate-limiter.js`
