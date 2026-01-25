---
title: messages-keys-and-config
tags:
  - "#mbb-spec"
  - "#core-systems"
  - "#messages"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# messages-keys-and-config

## Scope
- Формат ключей сообщений (v2) и типы.
- `messages-config` и базовый API для получения сообщений.

## When to Use
- Когда добавляете новые системные сообщения.
- При миграции legacy ключей.

## Key Rules
- **Короткие ключи**: `e.*`, `i.*`, `h.*`, `v.*`, `a.*`, `p.*`, `m.*`, `r.*`, `ai.*`.
- **Короткие типы**: `d/w/i/s` в конфиге, а не в ключе.
- **Legacy map**: старые ключи маппятся через `LEGACY_KEY_MAP`.

## Workflow
1) Добавить ключ в `messages-config`.
2) Определить `type` (`d/w/i/s`) и текст/детали.
3) При необходимости добавить legacy‑ключ в `LEGACY_KEY_MAP`.

## References
- `core/config/messages-config.js`
