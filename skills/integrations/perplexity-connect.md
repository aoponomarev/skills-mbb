---
title: integrations-perplexity-connect
tags: [#mbb-spec, #integrations, #ai, #perplexity]
dependencies: [integrations-ai-core]
mcp_resource: true
updated_at: 2026-01-24
---

# integrations-perplexity-connect

## Scope
- Использование Perplexity AI в проекте MBB.
- Получение и настройка API ключа Perplexity.
- Интеграция с `aiProviderManager`.

## When to Use
- При необходимости подключения или переключения на Perplexity AI.
- Для перевода tooltips, генерации новостей и AI-анализа текста.

## Key Rules
- Perplexity интегрирован через `core/api/perplexity.js`, `perplexity-provider.js` и `ai-provider-manager.js`.
- Используется стандартный формат Chat Completions API.
- API endpoint: `https://api.perplexity.ai/chat/completions`.
- API ключ вводится через настройки приложения: **Настройки → AI API**.
- Настройки Perplexity (ключ, модель) хранятся в кэше (`perplexity-api-key`, `perplexity-model`).
- Текущий провайдер хранится в кэше как `ai-provider` со значением `perplexity`.
- Список доступных моделей определяется в `core/config/app-config.js` (дефолт `sonar-pro`).

## Workflow
1) Зарегистрировать аккаунт на https://www.perplexity.ai/ и получить API ключ.
2) В приложении MBB: **Настройки → AI API → Perplexity AI**.
3) Ввести API ключ и выбрать модель (`sonar-pro` по умолчанию).
4) Нажать **Сохранить**.
5) Для использования: `await window.aiProviderManager.sendRequest([...])`.

## References
- Perplexity API документация: https://docs.perplexity.ai/
- `core/api/ai-provider-manager.js`
- `core/api/ai-providers/perplexity-provider.js`
- `core/api/perplexity.js`
- `core/config/app-config.js`
- `docs/doc-ai-providers.md`
