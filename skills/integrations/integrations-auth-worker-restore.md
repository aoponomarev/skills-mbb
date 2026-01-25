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
- Базовые bindings и маршруты Worker (D1, KV cache, proxy, datasets).

## When to Use
- При восстановлении окружения (Cloudflare Worker + D1 + OAuth).
- При миграции или повторном деплое авторизации.

## Key Rules
- **Секреты никогда не хранятся в репозитории**. Только через `wrangler secret`.
- **API токены и Client Secret** должны быть в защищенном хранилище.
- Все значения идентификаторов (Account ID, Database ID, Client ID) берутся из вашего аккаунта, не из кода.
- **KV `API_CACHE` обязателен** для прокси‑кеша внешних API (CoinGecko/Yahoo/Stooq).
- **Секреты** для восстановления брать из `MBB/secrets-backup.txt` (локально, без коммита).

## Workflow
1) **Cloudflare**: убедиться, что аккаунт и Worker созданы.
2) **D1**: проверить наличие базы и биндинга `DB`.
3) **KV Cache**: проверить биндинг `API_CACHE` в `wrangler.toml`.
4) **Wrangler**: настроить `wrangler.toml` и окружение.
5) **Secrets**: добавить секреты через `wrangler secret put`.
6) **Deploy**: `wrangler deploy`, затем health‑check.
7) **Client**: проверить настройки OAuth в `core/config/auth-config.js` и базовые endpoints.

## References
- `cloud/cloudflare/workers/wrangler.toml`
- `cloud/cloudflare/workers/src/index.js`
- `cloud/cloudflare/workers/src/api-proxy.js`
- `core/config/auth-config.js`
- `core/config/cloudflare-config.js`
- `docs/doc-file-protocol-oauth.md`
