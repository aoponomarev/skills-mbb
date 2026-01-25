---
title: integrations-postgres
tags:
  - "#mbb-spec"
  - "#integrations"
  - "#postgres"
  - "#cloud"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---

# integrations-postgres

## Scope
- Интеграция с PostgreSQL (включая Yandex Cloud).
- Клиент, синхронизация и конфигурация API слоя.

## Key Components
- `core/api/postgres-client.js`
- `core/api/postgres-sync-manager.js`
- `core/config/postgres-config.js`
- `cloud/yandex/functions/mbb-api/index.js`

## Key Rules
- Конфигурация подключения хранится централизованно.
- Синхронизация данных управляется отдельным менеджером.
- Функции API не содержат секретов в репозитории.

## Workflow
1) Обновить параметры в `postgres-config`.
2) Проверить клиент и синхронизацию.
3) Обновить/проверить API функцию в Yandex Cloud.

## References
- `core/api/postgres-client.js`
- `core/api/postgres-sync-manager.js`
- `core/config/postgres-config.js`
- `cloud/yandex/functions/mbb-api/index.js`
