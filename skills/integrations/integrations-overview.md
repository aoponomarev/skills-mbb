---
title: integrations-overview
tags: [#mbb-spec, #integrations, #architecture]
dependencies: []
mcp_resource: true
updated_at: 2026-01-26
---

# External Integrations Overview

> SSOT по стратегии и текущему статусу внешних интеграций MBB.

## Scope

- Integrations Overview functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

## Принципы и Стратегия
- **On-demand**: Интеграция по мере необходимости, без избыточности.
- **Resilience**: Отказоустойчивость (primary → secondary → local).
- **Geo-selection**: РФ/СНГ → Yandex Cloud, Global → Cloudflare.
- **Centralized Config**: Единый источник правды в `core/config/` и `.env`.

## Текущий Статус Реализации

### ✅ Реализовано
- **Auth**: Google OAuth через Cloudflare Workers.
- **AI**: YandexGPT (Yandex Cloud Functions) + Perplexity AI (fallback).
- **Storage**: Cloudflare D1 (Users/Portfolios).
- **Hosting**: GitHub Pages + CDN libs.
- **Automation**: n8n Community Edition (Local Docker + Volumes).

### ⚠️ В Процессе / Частично
- **Configuration**: Централизованное управление через `.env` и секреты n8n.
- **Caching**: Облачное кэширование (KV/R2) — ожидается интеграция.

### 🎯 Планируется (High Priority)
- **Integration Manager**: `core/api/integration-manager.js` для мониторинга и авто-переключения.
- **Unified Workers AI**: Динамический фолбэк между YandexGPT и Perplexity.
- **Backup**: Резервное копирование D1 в Object Storage.

## Команда "EI:" (External Insights)
Триггер для анализа новых интеграций: сравнение провайдеров, проектирование фолбэка, план реализации.

## References
- `core/config/`
- `.env`
- `docker-compose.yml`
- [Skill: integrations-n8n-local-setup]
