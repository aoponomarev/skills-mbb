---
title: cache-strategy
tags:
  - "#mbb-spec"
  - "#cache"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Cache Strategy functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.



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

## Troubleshooting: Кэш "не работает"

### Симптом: cacheHit: false при каждом запуске

**Возможные причины (не ошибки):**
1. **Первый запуск** — кэш пуст, данные загружаются из API
2. **TTL истёк** — данные устарели и были удалены (проверить `expiresAt < Date.now()`)
3. **Смена версии приложения** — версионированные ключи инвалидируются при изменении hash
4. **Hard reload (Ctrl+Shift+R)** — может очистить localStorage

**Как проверить:**
- Логировать `expiresAt` и `Date.now()` при `get()` — если `expiresAt < now`, данные истекли
- Проверить localStorage/IndexedDB в DevTools — есть ли ключ
- Сравнить версию приложения — изменился ли hash (см. `v:{hash}:{key}`)
- Повторный запуск без hard reload должен показать `cacheHit: true`

**Ключи, которые НЕ версионируются** (не инвалидируются при смене версии):
- Пользовательские данные: `portfolios`, `strategies`, `favorites`
- Настройки: `settings`, `theme`, `timezone`, API ключи

**Ключи, которые версионируются** (инвалидируются при смене версии):
- `icons-cache`, `coins-list`, `api-cache`, `market-metrics`, `crypto-news-state`, `stablecoins-list`

### Симптом: Данные не сохраняются в кэш

**Возможные причины:**
1. **Ключ не указан в storage-layers.js** — попадает в `hot` (localStorage) по умолчанию
2. **Переполнение localStorage** — лимит ~5MB, большие данные могут не сохраниться
3. **Ошибка в `set()`** — проверить возвращаемое значение (должно быть `true`)

**Как проверить:**
- `const result = await cacheManager.set(key, data)` — если `false`, ошибка сохранения
- Проверить размер данных: `JSON.stringify(data).length` — если >1MB, возможно переполнение
- Добавить ключ в соответствующий слой в `storage-layers.js`
