---
title: troubleshooting-yandex-access-binding-issue
tags: [#mbb-spec, #troubleshooting, #yandex-cloud, #iam]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# troubleshooting-yandex-access-binding-issue

## Scope
- Решение проблемы `PermissionDenied` при создании Yandex Cloud Function.
- Различие ролей на сервисный аккаунт и на папку.

## When to Use
- При ошибке `PermissionDenied` при создании Cloud Function.
- При настройке прав доступа для сервисных аккаунтов в Yandex Cloud.

## Key Rules
- **Роль `editor` должна быть назначена на папку**, а не на сервисный аккаунт.
- Проверять права через `yc resource-manager folder list-access-bindings <folder-id>`.
- В веб-интерфейсе: вкладка **"Каталог"** в разделе "Права доступа" папки.

## Workflow
1) Определить ID сервисного аккаунта и ID папки.
2) Проверить текущие права на папке через CLI или веб-интерфейс.
3) Назначить роль `editor` на папку для сервисного аккаунта через CLI:
   ```bash
   yc resource-manager folder add-access-binding <folder-id> \
     --role editor \
     --subject serviceAccount:<service-account-id>
   ```
4) Повторно проверить права и убедиться, что роль назначена корректно.
5) Если ошибка остается — подождать 1-2 минуты, обновить страницу, попробовать снова.

## References
- Документация Yandex Cloud IAM: https://yandex.cloud/ru/docs/iam/
- `cloud-yandex-cloud-function-steps-guide.md`
- `cloud-yandex-cloud-function-code.md`
