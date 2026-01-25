---
title: integrations-cloudflare-testing
tags: [#mbb-spec, #integrations]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# integrations-cloudflare-testing

> Источник: `docs/doc-cloudflare-testing.md`

## Предусловия

- Worker доступен
- OAuth Client ID настроен
- D1 создана и схема применена
- Feature flags включены

## Чеклист (сокращенно)

- OAuth flow: редирект, callback, токены
- Workers endpoints: health, auth/callback, protected APIs
- API клиенты: portfolios, datasets, auth
- UI компоненты: auth-button, portfolios-manager
- Интеграция: полный flow + feature flags
