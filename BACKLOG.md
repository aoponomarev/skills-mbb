# Skills Backlog (MBB-Specific)

Этот файл служит буфером для новых кандидатов в MBB-специфичные Skills.
Агенты добавляют сюда предложения, а человек (USER) одобряет их и переносит в основной реестр.

## Candidate Pipeline
1. **Capture**: Агент выявляет потребность (Create/Update/Merge/Deprecate).
2. **Draft**: n8n генерирует черновик в папке drafts/.
3. **Review**: Человек (USER) просматривает список.
4. **Promote**: Если одобрено, изменения вносятся в основной реестр, а запись архивируется.

## Action Types
- `create`: создать новый Skill
- `update`: обновить существующий
- `merge`: объединить Skills
- `split`: разделить Skill
- `deprecate`: архивировать Skill
- `move`: перенести Skill между `skills/` и `skills-mbb/`

## Entry Format
`- [action=<action>] [status=pending] title="<Title>" | scope="<Scope>" | skill_id="<existing_id>" | changes="<description>" | tags=[tag1, tag2] | priority="<low|medium|high|critical>" | context="<cursor_context>" | timestamp=<ISO8601>`

---

## 🚀 Candidates (Pending Review)

- [status=pending] title="skills-mcp basic smoke test" | scope="Проверка записи в BACKLOG через MCP" | tags=[mcp, skills] | source="manual-test" | priority="low" | timestamp=2026-01-25T13:03:37.046Z

---

## ✅ Archive (Recently Promoted)

- [status=promoted] title="integrations-overview" | action="merge" | source="integrations-status, integrations-strategy" | priority="medium" | timestamp=2026-01-26T17:30:00Z
- [status=promoted] title="integrations-n8n-local-setup" | action="create" | source="n8n-migration" | priority="medium" | timestamp=2026-01-26T12:50:46Z
- [status=promoted] title="integrations-data-providers" | scope="Единый интерфейс и конфигурация провайдеров данных (CoinGecko и др.)" | tags=[integrations, data, providers] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="integrations-rate-limiting" | scope="Централизованное ограничение запросов к API" | tags=[integrations, api, rate-limit] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="integrations-postgres" | scope="Интеграция и синхронизация с PostgreSQL (Yandex Cloud)" | tags=[integrations, postgres, cloud] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="libs-metadata-generation" | scope="Генерация и загрузка метаданных монет (coins.json и пр.)" | tags=[libs, metadata, data] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="components-icon-manager" | scope="Единый источник URL иконок с приоритетом CDN и fallback" | tags=[components, icons, cdn] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="core-systems-auto-coin-sets" | scope="Автоматическое формирование и обновление наборов монет" | tags=[core-systems, data, automation] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
- [status=promoted] title="core-systems-workspace-config" | scope="ЕИП для настроек рабочей зоны" | tags=[core-systems, config] | source="header-audit" | priority="medium" | timestamp=2026-01-25T14:10:00.000Z
