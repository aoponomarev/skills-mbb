---
title: integrations-cloudflare-core
tags:
  - "#mbb-spec"
  - "#integrations"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Integrations Cloudflare Core functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.



# integrations-cloudflare-core

> Источник: `docs/doc-cloudflare.md`

## Инфраструктура

- Workers (`cloud/cloudflare/workers/src/`)
- D1 (users, portfolios)
- R2 (отложено)
- KV (API proxy cache)

## Workers

- URL: `https://mbb-api.ponomarev-ux.workers.dev`
- Health: `/health`
- Роутер: `cloud/cloudflare/workers/src/index.js`

## D1

- DB: `mbb-database`
- ID: `887a3f58-98c2-41a4-a512-8dcdaea751e8`
- Схема: `cloud/cloudflare/workers/schema.sql`

## OAuth

- Клиент: `core/api/cloudflare/auth-client.js`
- Сервер: `cloud/cloudflare/workers/src/auth.js`
- Конфиг: `core/config/auth-config.js`

## Деплой

- `wrangler login`
- `wrangler deploy`
- Secrets: `GOOGLE_CLIENT_SECRET`, `JWT_SECRET`
