---
title: integrations-rate-limiting
tags:
  - "#mbb-spec"
  - "#integrations"
  - "#rate-limit"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---
## When to Use

- При необходимости работы с данным компонентом или функционалом.



# integrations-rate-limiting

## Scope
- Централизованное управление лимитами запросов к внешним API.

## Key Components
- `core/api/rate-limiter.js` — адаптивный rate limiter с token bucket
- `core/config/data-providers-config.js` — конфигурация лимитов для провайдеров

## Архитектура RateLimiter

### Два API (для обратной совместимости)

**Функциональный API (старый):**
```javascript
window.rateLimiter.waitBeforeRequest()
window.rateLimiter.increaseTimeout()
window.rateLimiter.decreaseTimeout()
```

**Класс RateLimiter (новый, ЕИП):**
```javascript
const limiter = window.RateLimiter.getOrCreate('coingecko', 15, 0.5);
await limiter.waitForToken();
limiter.increaseTimeout();  // при 429
limiter.decreaseTimeout();  // при успехе
```

### Именованные экземпляры (ЕИП)
`RateLimiter.getOrCreate(key, rpm, rps)` возвращает **общий экземпляр** для ключа.
Все компоненты, использующие один ключ (напр. 'coingecko'), разделяют один лимитер.

## Key Rules

1. **Лимиты задаются централизованно** в `data-providers-config.js`
2. **Всегда вызывать `decreaseTimeout()` при успехе** — иначе таймаут только растёт
3. **Использовать `buildUrl()` для proxy** — на `file://` прямые запросы к API не работают (CORS)
4. **Один лимитер на API** — использовать `getOrCreate()` с одинаковым ключом

## CoinGecko: Текущие лимиты

**Demo API (бесплатный):** 30 req/min
**Наша конфигурация:** 15 req/min, 0.5 req/sec (консервативно)

```javascript
// data-providers-config.js
rateLimit: {
    requestsPerMinute: 15,
    requestsPerSecond: 0.5  // 1 запрос в 2 секунды
}
```

## Troubleshooting: 429 Too Many Requests

### Симптом: Частые 429 ошибки от CoinGecko

**Возможные причины:**
1. **Параллельные запросы** — несколько компонентов запрашивают API одновременно
2. **Кэш не работает** — данные не сохраняются, каждый раз идёт запрос к API
3. **Лимитер не используется** — запросы идут без `waitForToken()`
4. **decreaseTimeout() не вызывается** — таймаут накапливается и не сбрасывается

**Как проверить:**
- Добавить логирование в `waitForToken()` и `increaseTimeout()`
- Проверить, что все методы провайдера используют `this.rateLimiter.waitForToken()`
- Убедиться, что при успехе вызывается `decreaseTimeout()`

### Симптом: 429 при первом запуске

**Это нормально**, если:
- Кэш пуст (первый запуск или после очистки)
- TTL истёк (данные устарели)
- Версия приложения изменилась (версионированные ключи инвалидированы)

При повторном запуске без hard reload 429 не должно быть (данные из кэша).

## Чеклист для нового API endpoint

1. [ ] Использовать `RateLimiter.getOrCreate(key)` с правильным ключом
2. [ ] Вызывать `await limiter.waitForToken()` перед каждым fetch
3. [ ] При 429: `limiter.increaseTimeout()`
4. [ ] При успехе: `limiter.decreaseTimeout()`
5. [ ] Использовать `buildUrl()` для поддержки proxy на `file://`
6. [ ] Добавить обработку ошибок и fallback на кэш

## References
- `core/api/rate-limiter.js`
- `core/config/data-providers-config.js`
- `core/api/data-providers/coingecko-provider.js`
- `core/api/coingecko-stablecoins-loader.js`
