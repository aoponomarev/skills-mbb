---
title: cache-strategy
tags:
  - "#mbb-spec"
  - "#cache"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# cache-strategy

> Источник: `docs/doc-cache.md` (архитектура, слои, TTL, стратегии)

## Архитектура кэширования

Единая система через `core/cache/cache-manager.js` скрывает детали хранилищ, обеспечивает версионирование, миграции и стратегии кэширования.

**Ключевые файлы:**
- `cache-manager.js`, `cache-config.js`, `storage-layers.js`
- `cache-migrations.js`, `cache-cleanup.js`, `cache-indexes.js`

## Слои хранения

**Hot (localStorage, ≤5MB):**
- `settings`, `theme`, `timezone`, `favorites`, `ui-state`, `active-tab`
- `icons-cache`

**Warm (IndexedDB, ≤50MB):**
- `coins-list`, `market-metrics`, `api-cache`

**Cold (IndexedDB, ≤500MB):**
- `time-series`, `history`, `portfolios`, `strategies`, `correlations`

## TTL (Time To Live)

**С TTL:**
- `icons-cache`: 1 час
- `coins-list`: 1 день
- `market-metrics`: 1 час
- `api-cache`: 5 минут
- `time-series`: 1 час
- `history`: 1 день
- `crypto-news-cache-max-age`: 24 часа
- `market-update-fallback`: 3 часа
- `market-update-delay-max`: 24 часа

**Без TTL:**
- пользовательские данные, настройки, API ключи, язык

## Стратегии кэширования

- **cache-first:** `icons-cache`, `coins-list`
- **network-first:** `market-metrics`, `api-cache`
- **stale-while-revalidate:** `time-series`, `history`
- **cache-only:** `portfolios`, `strategies`, `settings`, `theme`, `timezone`, `favorites`, `ui-state`, API ключи
