# architecture-versioning

> Источник: `docs/doc-architect.md` (раздел "Версионирование приложения")

## Версионирование приложения

**Назначение:** Привязка кэша и стилизации к версии приложения для автоматической инвалидации при обновлении.

**Реализация:**
- CSS‑класс на body: `app-version-{hash}`
- Версионирование ключей кэша: префикс `v:{hash}:`
- Автоматическая очистка старых ключей при инициализации

**Версионируемые ключи (автоматически):**
- `icons-cache`, `coins-list`, `api-cache`, `market-metrics`, `crypto-news-state`

**Неверсионируемые ключи (пользовательские данные):**
- `settings`, `theme`, `timezone`, `favorites`, `ui-state`
- `yandex-api-key`, `yandex-model`, `perplexity-api-key`, `perplexity-model`, `translation-language`, `ai-provider`
- `portfolios`, `strategies`, `time-series`, `history`

## Критерии применения версионирования кэша

**Версионировать, если:**
- Данные из внешних API (структура может меняться)
- Данные парсятся из ответов API
- Структура зависит от версии приложения
- Устаревшие данные могут вызвать ошибки

**Не версионировать, если:**
- Пользовательские данные и настройки
- UI‑состояние
- Данные с миграциями схем

## API

- `appConfig.getVersionHash()` — хэш версии
- `appConfig.getVersionClass()` — CSS‑класс версии
- `cacheManager.clearOldVersions()` — очистка старых ключей
- `cacheManager.getVersionedKey(key, useVersioning)` — получение версионированного ключа

## Файлы

- `core/config/app-config.js` (CONFIG.version)
- `app/app-ui-root.js` (установка класса версии)
- `core/cache/cache-manager.js` (версионирование)
- `shared/utils/hash-generator.js` (хэш)
