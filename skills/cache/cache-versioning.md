---
title: cache-versioning
tags: [#mbb-spec, #cache]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# cache-versioning

> Источник: `docs/doc-cache.md` (версионирование приложения)

## Назначение

Авто‑инвалидация кэша при обновлении версии через префикс `v:{hash}:{key}`.

## Версионируемые ключи

- `icons-cache`
- `coins-list`
- `api-cache`
- `market-metrics`
- `crypto-news-state`

**Особый случай:** `tooltips-{provider}-{versionHash}-{language}` — версия в имени ключа.

## НЕ версионируются

- `settings`, `theme`, `timezone`, `favorites`, `ui-state`
- `yandex-api-key`, `perplexity-api-key`, `yandex-model`, `perplexity-model`, `ai-provider`
- `portfolios`, `strategies`, `time-series`, `history`
- `translation-language`

## Критерии

**Версионировать, если:**
- данные из внешних API
- данные парсятся из ответов
- структура зависит от версии приложения
- ошибки в старых данных опасны

**Не версионировать, если:**
- пользовательские данные/настройки
- UI‑состояние
- данные с миграциями схем

## Реализация

- `core/cache/cache-manager.js` (`getVersionedKey`, `clearOldVersions`)
