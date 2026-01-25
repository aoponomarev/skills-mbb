---
title: cloud-yandex-cloud-function-code
tags: [#mbb-spec, #cloud, #yandex-cloud, #serverless]
dependencies: [troubleshooting-yandex-access-binding-issue, troubleshooting-yandex-cors-troubleshooting, cloud-yandex-get-api-key]
mcp_resource: true
updated_at: 2026-01-24
---

# cloud-yandex-cloud-function-code

## Scope
- Исправленный код Yandex Cloud Function для работы с YandexGPT API.
- Устранение проблемы с `require('node-fetch')`.
- Обработка CORS, валидация запросов, извлечение API-ключа.

## When to Use
- При создании или обновлении Yandex Cloud Function для проксирования YandexGPT API.
- При работе с `Node.js 18+` в Cloud Functions.

## Key Rules
- **Использовать встроенный `fetch`** (Node.js 18+), не подключать `node-fetch`.
- **Обрабатывать `OPTIONS`** запросы для CORS (статус `204`).
- **API-ключ** брать из переменных окружения (`process.env.YANDEX_API_KEY`) или из тела запроса.
- **Валидация** `modelUri` и `messages` обязательна.

## Workflow
1) Использовать предоставленный код для Cloud Function.
2) Убедиться, что `YANDEX_API_KEY` настроен как переменная окружения функции.
3) Включить переключатель "Публичная функция" в Yandex Cloud Console.
4) Тестировать через приложение или `curl`.

## References
- `cloud-yandex-cloud-function-steps-guide.md`
- `troubleshooting-yandex-cors-troubleshooting.md`
- `cloud-yandex-get-api-key.md`
