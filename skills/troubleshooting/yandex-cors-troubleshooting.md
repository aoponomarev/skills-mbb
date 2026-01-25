---
title: troubleshooting-yandex-cors-troubleshooting
tags:
  - "#mbb-spec"
  - "#troubleshooting"
  - "#yandex-cloud"
  - "#cors"
dependencies: [cloud-yandex-cloud-function-code, cloud-yandex-cloud-function-steps-guide, cloud-yandex-get-api-key]
mcp_resource: true
updated_at: 2026-01-24
---

# troubleshooting-yandex-cors-troubleshooting

## Scope
- Устранение проблем с CORS в Yandex Cloud Functions.
- Проверка публичного доступа, логов функции, тестирование через `curl`.

## When to Use
- При возникновении ошибки CORS типа "Response to preflight request doesn't pass access control check".
- При отладке взаимодействия клиент-сервер с Yandex Cloud Function.

## Key Rules
- **Функция должна быть публичной** (переключатель в консоли ВКЛЮЧЕН).
- **Проверять логи функции** на наличие `OPTIONS` запросов и ошибок выполнения.
- Использовать `curl` для прямого тестирования `OPTIONS` и `POST` запросов, чтобы обойти ограничения браузера.
- **Определение HTTP метода** в коде функции должно быть устойчивым (`event?.httpMethod || event?.requestContext?.httpMethod || ...`).

## Workflow
1) Убедиться, что функция `yandexgpt-proxy` публична.
2) Проверить логи функции в Yandex Cloud Console на наличие запросов и ошибок.
3) Протестировать `OPTIONS` и `POST` запросы к функции через `curl` (с заголовками `Origin` и `Access-Control-Request-*`).
4) Если проблема в определении метода — проверить и скорректировать код функции.
5) Если функция падает на OPTIONS — проверить синтаксис и формат возвращаемого значения в коде.

## References
- `cloud-yandex-cloud-function-code.md`
- `cloud-yandex-cloud-function-steps-guide.md`
- `cloud-yandex-get-api-key.md`
