---
title: integrations-auth-worker-restore
tags: [#mbb-spec, #integrations, #cloudflare, #auth]
dependencies: [integrations-cloudflare-core, integrations-api-proxy]
mcp_resource: true
updated_at: 2026-01-24
---

# integrations-auth-worker-restore

## Scope
- Полный процесс восстановления Cloudflare Worker и Google OAuth авторизации.
- Без хранения секретов в коде.

## When to Use
- При восстановлении окружения (Cloudflare Worker + D1 + OAuth).
- При миграции или повторном деплое авторизации.

## Key Rules
- **Секреты никогда не хранятся в репозитории**. Только через `wrangler secret`.
- **API токены и Client Secret** должны быть в защищенном хранилище.
- Все значения идентификаторов (Account ID, Database ID, Client ID) берутся из вашего аккаунта, не из кода.

## Workflow
1) **Cloudflare**: убедиться, что аккаунт и Worker созданы.
2) **D1**: проверить наличие базы и биндинга `DB`.
3) **Wrangler**: настроить `wrangler.toml` и окружение.
4) **Secrets**: добавить секреты через `wrangler secret put`.
5) **Deploy**: `wrangler deploy`, затем health‑check.
6) **Client**: проверить настройки OAuth в `core/config/auth-config.js`.

## References
- `cloud/cloudflare/workers/wrangler.toml`
- `cloud/cloudflare/workers/src/index.js`
- `core/config/auth-config.js`
- `docs/doc-file-protocol-oauth.md`
