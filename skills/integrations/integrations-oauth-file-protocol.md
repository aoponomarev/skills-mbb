---
title: integrations-oauth-file-protocol
tags:
  - "#mbb-spec"
  - "#integrations"
  - "#auth"
  - "#oauth"
  - "#file-protocol"
dependencies: [integrations-auth-worker-restore]
mcp_resource: true
updated_at: 2026-01-24
---

# integrations-oauth-file-protocol

## Scope
- Обработка OAuth callback при запуске приложения через `file://`.
- Механизм передачи токена из popup‑окна обратно в приложение.

## When to Use
- При отладке OAuth в локальном режиме `file://`.
- Если редирект обратно в локальный файл невозможен.

## Key Rules
- **Popup обязателен**: OAuth должен открываться через `window.open()` для `postMessage`.
- **Fallback обязателен**: при блокировке popup — сохранять токен в `localStorage`.
- **Безопасность**: секреты и токены не хранятся в репозитории.

## Workflow
1) При `file://` сохранить `client_url` в `state`.
2) OAuth callback возвращает HTML с JS для `postMessage` и `localStorage`.
3) Приложение слушает `message` и сохраняет токен.
4) При необходимости предложить запуск локального HTTP сервера вместо `file://`.

## References
- `core/api/cloudflare/auth-client.js`
- `cloud/cloudflare/workers/src/auth.js`
- `app/components/auth-button.js`
