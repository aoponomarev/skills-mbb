---
title: cloud-yandex-cloud-function-steps-guide
tags: [#mbb-spec, #cloud, #yandex-cloud, #serverless]
dependencies: [cloud-yandex-cloud-function-code, troubleshooting-yandex-cors-troubleshooting, cloud-yandex-get-api-key]
mcp_resource: true
updated_at: 2026-01-24
---

# cloud-yandex-cloud-function-steps-guide

## Scope
- Пошаговое руководство по созданию версии Yandex Cloud Function.
- Настройка среды выполнения, вставка кода, переменные окружения, таймаут.

## When to Use
- При первом развёртывании Yandex Cloud Function.
- При обновлении или создании новой версии функции.

## Key Rules
- **Выбирать Node.js 22** (или актуальную версию) и **снимать галочку с "Добавить файлы с примерами кода"**.
- **Использовать исправленный код** из `cloud-yandex-cloud-function-code.md`.
- **Настроить `YANDEX_API_KEY`** как переменную окружения функции.
- **Таймаут функции: 30 секунд**.
- Функция должна быть **публичной** для внешних вызовов.

## Workflow
1) Открыть страницу функции в Yandex Cloud Console.
2) Выбрать среду выполнения `Node.js 22`.
3) Вставить исправленный код функции.
4) Настроить переменную окружения `YANDEX_API_KEY`.
5) Установить таймаут `30` секунд.
6) Сохранить и дождаться деплоя.
7) Проверить работу функции через приложение или `curl`.

## References
- `cloud-yandex-cloud-function-code.md`
- `troubleshooting-yandex-cors-troubleshooting.md`
- `cloud-yandex-get-api-key.md`
