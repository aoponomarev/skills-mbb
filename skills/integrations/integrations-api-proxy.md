---
title: integrations-api-proxy
tags:
  - "#mbb-spec"
  - "#integrations"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Integrations Api Proxy functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# integrations-api-proxy

> Источник: `docs/doc-cloudflare.md` (раздел "API Proxy")

## Назначение

Cloudflare Worker проксирует внешние API для работы на `file://` и кэширует ответы в KV.

## Поддерживаемые API

- CoinGecko: `/api/coingecko/*`
- Yahoo Finance: `/api/yahoo-finance/*`
- Stooq: `/api/stooq/*`

## KV кэш

Namespace: `API_CACHE`

TTL по маршрутам:
- CoinGecko `/coins/markets` — 5 мин
- CoinGecko `/coins/list` — 24 часа
- CoinGecko `/simple/price` — 1 мин
- CoinGecko `/global` — 1 час
- Yahoo `/v8/finance/chart` — 1 час
- Stooq `/q/d/l/` — 1 час

## Конфигурация

- `core/config/cloudflare-config.js` → `getApiProxyEndpoint()`
- `cloud/cloudflare/workers/src/api-proxy.js`
