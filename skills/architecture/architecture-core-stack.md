---
title: architecture-core-stack
tags: [#mbb-spec, #architecture]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# architecture-core-stack

> Источник: `docs/doc-architect.md` (раздел "Технологический стек")

## Технологический стек

### Среда исполнения
- Приложение развёрнуто на GitHub Pages: только фронтенд, без серверного кода, корректные относительные пути и загрузка ресурсов по HTTPS (без `content://`).

### Фреймворки и UI
- Основной стек: Vue.js + Bootstrap для стилизации.
- Приоритет Bootstrap-классов и утилит; кастомный CSS, inline-стили и `<style>`-блоки использовать минимально.

### Работа фронтенда
- Внешние HTTP-запросы не запускать вне `#app`, пока сплэш-экран не разблокирован.
- Компоненты и разметка, управляемые Vue, должны располагаться внутри `<div id="app">`.

### Проксирование внешних API на file://

**КРИТИЧЕСКИ ВАЖНО:** При работе на `file://` протоколе все запросы к внешним API **ОБЯЗАТЕЛЬНО** проксируются через Cloudflare Worker для обхода CORS ограничений браузера. **Обоснование:** Браузеры блокируют CORS запросы с `file://` к внешним доменам по соображениям безопасности. Cloudflare Worker выступает в роли прокси-сервера, который делает запросы от имени клиента и возвращает данные с корректными CORS headers. Это позволяет приложению работать локально без необходимости запуска HTTP-сервера, сохраняя при этом доступ ко всем внешним API (CoinGecko, Yahoo Finance, Stooq и др.). На HTTP/HTTPS протоколах запросы идут напрямую к API без прокси.

**Архитектура проксирования:**
- **Cloudflare Worker:** `https://mbb-api.ponomarev-ux.workers.dev/api/{service}/*`
- **Поддерживаемые сервисы:** `coingecko`, `yahoo-finance`, `stooq`
- **KV кэширование:** Все запросы кэшируются в Cloudflare KV с настраиваемым TTL (5 мин - 24 часа)
- **Автоматическое переключение:** Клиентские модули автоматически используют proxy на `file://` и прямые запросы на HTTP/HTTPS

**Конфигурация:**
- `core/config/cloudflare-config.js` — proxy endpoints и функция `getApiProxyEndpoint(apiType, path, params)`
- `cloud/cloudflare/workers/src/api-proxy.js` — универсальный прокси handler с KV кэшированием
- `cloud/cloudflare/workers/wrangler.toml` — KV namespace binding `API_CACHE`

**Реализация в модулях:**
Все модули, работающие с внешними API, должны использовать метод `buildUrl()` или аналогичный, который автоматически определяет протокол и выбирает прямой запрос или proxy:

```javascript
buildUrl(pathWithQuery) {
    const isFile = window.location && window.location.protocol === 'file:';

    // Если file:// — используем Cloudflare Worker proxy
    if (isFile && window.cloudflareConfig) {
        const [path, query] = pathWithQuery.split('?');
        const params = query ? Object.fromEntries(new URLSearchParams(query)) : {};
        return window.cloudflareConfig.getApiProxyEndpoint('coingecko', path, params);
    }

    // Иначе — прямой запрос к API
    return `${this.config.baseUrl}${pathWithQuery}`;
}
```

**ЗАПРЕЩЕНО:**
- Блокировать запросы на `file://` с early return без использования proxy
- Пропускать обновления данных на `file://` с сообщениями "CORS ограничение"
- Использовать публичные CORS прокси (ненадежны, медленны, имеют свои rate limits)

**Документация:**
- `docs/doc-cloudflare.md` (раздел "§ API Proxy") — детальное описание архитектуры proxy
- `cloud/cloudflare/workers/DEPLOY_INSTRUCTIONS.md` — инструкции по деплою Worker с KV namespace

### Перенос кода из первоисточников и старых версий
- Сохранять имена, семантику переменных/функций, полезные комментарии.
- Избегать конфликтов имён, комментировать новые переменные и решения.
- При возникновении конфликтов имен переменных — сообщать пользователю с достаточной детализацией, предлагая варианты решения.
- При переносе кода из первоисточников (примеров кода из других проектов) использовать переменные первоисточников в приоритете.
- Переносить комментарии первоисточников с сохранением их сути и контекста, адаптируя при необходимости. Всегда комментировать переменные при их объявлении, указывая их назначение и семантику для понимания другим разработчиком или ИИ-агентом.
