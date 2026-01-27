---
title: integrations-cloudflare-plan
tags:
  - "#mbb-spec"
  - "#integrations"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Integrations Cloudflare Plan functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.



# integrations-cloudflare-plan

> Источник: `docs/doc-cloudflare-integration-plan.md`

## Этапы интеграции

1. Инфраструктура Cloudflare (Workers, D1, OAuth)
2. Конфигурация и SSOT (`auth-config`, `cloudflare-config`)
3. OAuth клиент (браузер)
4. Workers серверные endpoints
5. API клиенты (portfolios, datasets)
6. UI авторизации
7. UI портфелей
8. Интеграция с приложением (feature flags)
9. Тестирование и отладка
10. Документация и финализация

## Статусы

Все этапы 1–10 выполнены, кроме отложенного R2 (требуется платежный метод).
