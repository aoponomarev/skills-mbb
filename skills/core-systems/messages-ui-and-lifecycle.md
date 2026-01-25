---
title: messages-ui-and-lifecycle
tags:
  - "#mbb-spec"
  - "#core-systems"
  - "#messages"
  - "#ui"
dependencies: [messages-keys-and-config, messages-translator]
mcp_resource: true
updated_at: 2026-01-24
---

# messages-ui-and-lifecycle

## Scope
- `AppMessages` store, UI компоненты, жизненный цикл сообщений.
- Критичные нюансы (spread, key, Vue 3 локальная регистрация).

## When to Use
- При создании/отображении сообщений в UI.
- При интеграции логгера и переводчика.

## Key Rules
- **Всегда сохранять `key`** при создании сообщений.
- **Использовать spread (`...msg`)** при пересоздании сообщений после перевода.
- В Vue 3 использовать **локальную** регистрацию `cmp-system-message`.

## Workflow
1) Получить сообщение через `messagesConfig.get(key, params)`.
2) Добавить в `AppMessages.push({ ...msg, scope })`.
3) UI отображает `cmp-system-messages`.
4) При смене языка — пересборка сообщений через `messagesTranslator`.

## References
- `core/api/app-messages.js`
- `app/components/system-messages.js`
- `app/components/system-message.js`
