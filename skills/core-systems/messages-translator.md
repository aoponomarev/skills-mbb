---
title: messages-translator
tags: [#mbb-spec, #core-systems, #messages, #i18n]
dependencies: [messages-keys-and-config]
mcp_resource: true
updated_at: 2026-01-24
---

# messages-translator

## Scope
- Формат переводов и правила парсинга.
- `messagesTranslator` API и процесс перевода.

## When to Use
- При добавлении новых переводимых сообщений.
- При отладке смены языка.

## Key Rules
- Формат переводов: `KEY|TEXT|DETAILS`.
- Хранение: `localStorage` (`tr-{lang}`) с массивом `[text, details]`.
- В ответах AI **placeholder** (`{name}`) не менять.

## Workflow
1) `messagesTranslator.init(lang)` при загрузке.
2) `updateLanguage(lang)` при смене языка.
3) Если перевода нет — запрос к AI, парсинг и кэш.
4) `updateDisplayedMessages()` пересобирает сообщения.

## References
- `core/api/messages-translator.js`
- `core/api/ai-provider-manager.js`
