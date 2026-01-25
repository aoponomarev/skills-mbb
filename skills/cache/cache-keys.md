---
title: cache-keys
tags: [#mbb-spec, #cache]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# cache-keys

> Источник: `docs/doc-cache.md` (правила принятия решений)

## Алгоритм добавления ключа

### 1) Выбор слоя хранения

- **Hot** (localStorage): <100KB, частый доступ, простые структуры
- **Warm** (IndexedDB): 100KB–10MB, частый доступ, структурированные данные
- **Cold** (IndexedDB): >10MB, редкий доступ, большие структуры

Добавлять ключ в `core/cache/storage-layers.js` → `LAYERS.{layer}.keys`

### 2) Версионирование

- Внешние API / парсинг / завязано на версию → версионировать
- Пользовательские данные / UI‑состояние / миграции → не версионировать

Добавлять ключ в `core/cache/cache-manager.js` → `versionedKeys`

### 3) TTL

- Устаревает → задать TTL
- Пользовательские данные → без TTL

Добавлять в `core/cache/cache-config.js` → `TTL`

### 4) Стратегия

- cache-first → стабильные данные
- network-first → критична актуальность
- stale-while-revalidate → баланс
- cache-only → только локальные данные

Добавлять в `core/cache/cache-config.js` → `STRATEGIES`

### 5) Preload Strategy

Использовать при жестких rate limits (CoinGecko): загружать максимальный набор один раз и нарезать из кэша.

Ключи:
- `top-coins-by-market-cap`
- `top-coins-by-volume`

### 6) Версия схемы (пользовательские данные)

Если пользовательские данные меняют структуру:
- `core/cache/cache-config.js` → `VERSIONS`
- `core/cache/cache-migrations.js` → миграции
