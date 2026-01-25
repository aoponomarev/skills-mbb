---
title: cloud-yandex-get-api-key
tags: [#mbb-spec, #cloud, #yandex-cloud, #api-key]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# cloud-yandex-get-api-key

## Scope
- Пошаговое руководство по получению API-ключа Yandex Cloud.
- Использование раздела IAM и сервисных аккаунтов.

## When to Use
- При необходимости получить новый API-ключ Yandex Cloud.
- При настройке Yandex Cloud Functions или других сервисов, требующих API-ключ.

## Key Rules
- **API-ключ показывается только один раз** при создании. Скопировать немедленно.
- Ключ имеет формат `AQVN...`.
- После получения ключ используется как переменная окружения `YANDEX_API_KEY` в Cloud Function.
- Если ключ утерян — нужно создать новый, старый восстановить нельзя.

## Workflow
1) В Yandex Cloud Console перейти в раздел **IAM**.
2) Выбрать вкладку **"Сервисные аккаунты"** и найти аккаунт `mbb`.
3) На странице сервисного аккаунта в разделе **"Ключи"** нажать **"Создать API-ключ"**.
4) **Скопировать ключ немедленно** и сохранить в безопасном месте.
5) В Cloud Function настроить `YANDEX_API_KEY` как переменную окружения.

## References
- `cloud-yandex-cloud-function-steps-guide.md`
- `cloud-yandex-cloud-function-code.md`
