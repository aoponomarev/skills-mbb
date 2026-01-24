# integrations-ai-core

> Источник: `docs/doc-ai-providers.md`

## Архитектура

- `core/api/ai-provider-manager.js`
- `core/api/ai-providers/base-provider.js`
- Провайдеры: `yandex-provider`, `perplexity-provider`

## Принципы

- Единый интерфейс `BaseAIProvider`
- Абстракция различий в форматах запросов/ответов
- Переключение без изменения кода приложения

## Механизм переключения

- Текущий провайдер в кэше: `ai-provider`
- API ключи: `yandex-api-key`, `perplexity-api-key`
- Модели: `yandex-model`, `perplexity-model`
- Переключение через `aiProviderManager.setProvider()`

## Кэширование по провайдеру

Ключи переводов включают имя провайдера:
`tooltips-{provider}-{versionHash}-{language}`

## Добавление провайдера (кратко)

1. Новый класс провайдера
2. Регистрация в `modules-config.js`
3. Добавление в `ai-provider-manager.js`
4. Настройки в `app/components/ai-api-settings.js`
5. Defaults в `core/config/app-config.js`
