---
title: process-skill-pipeline
tags: [#process, #skills, #automation, #n8n, #continue-cli]
dependencies: [integrations-continue-cli-mistral, integrations-n8n-local-setup]
mcp_resource: true
updated_at: 2026-01-27
---

# Process: Automated Skill Generation Pipeline

## Scope
- Автоматическая генерация Skill документов из BACKLOG.md.
- Интеграция Continue CLI + Mistral для генерации контента.
- Cron-based execution vs n8n workflows.

## When to Use
- При добавлении новых записей в BACKLOG.md.
- Для автоматизации создания черновиков Skills.
- Как референс для построения подобных пайплайнов.

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  BACKLOG.md     │────▶│ skill-processor  │────▶│ Continue CLI    │
│  (source)       │     │ (every 5 min)    │     │ + Mistral       │
└─────────────────┘     └──────────────────┘     └────────┬────────┘
                                                          │
                                                          ▼
                                               ┌─────────────────────┐
                                               │ skills-mbb/drafts/  │
                                               │ (output)            │
                                               └─────────────────────┘
```

## BACKLOG Entry Format

```markdown
- [action=create] [status=pending] title="Skill Title" | scope="Description" | tags=[tag1, tag2] | priority="high" | context="Additional context" | timestamp=2026-01-27T10:00:00Z
```

**Обязательные поля:**
- `action=create` — тип действия
- `status=pending` — статус для обработки
- `title="..."` — название skill
- `scope="..."` — описание области
- `tags=[...]` — теги
- `priority="..."` — приоритет
- `timestamp=...` — ISO8601 дата (без кавычек!)

## skill-processor.js

Автономный Node.js скрипт, который:
1. Читает BACKLOG.md
2. Находит записи с `[action=create] [status=pending]`
3. Вызывает Continue CLI для генерации
4. Сохраняет результат в `drafts/`

```javascript
// Regex для парсинга
const regex = /- \[action=create\] \[status=pending\] title="([^"]+)" \| scope="([^"]+)".*?tags=\[([^\]]+)\].*?priority="([^"]+)".*?timestamp=(\S+)/g;

// Вызов Continue CLI
const result = await fetch('http://localhost:3000/prompt', {
  method: 'POST',
  body: JSON.stringify({ prompt })
});

// Сохранение через /save endpoint
await fetch('http://localhost:3000/save', {
  method: 'POST',
  body: JSON.stringify({ filepath, content })
});
```

## Запуск

### Ручной запуск
```bash
docker exec continue-cli node /workspace/mbb/mcp/continue-wrapper/skill-processor.js
```

### Автоматический (cron в контейнере)
```yaml
# docker-compose.yml
command: >
  sh -c "node server.js &
  while true; do sleep 300; node skill-processor.js; done"
```

## Почему не n8n workflow?

В ходе экспериментов выявлены проблемы с n8n:

1. **Code node API изменился в n8n 2.x**
   - `$input.first()` не работает как ожидалось
   - `items[0]` тоже ведёт себя непредсказуемо
   
2. **executeCommand node отключен**
   - В n8n 2.0+ требует `N8N_NODES_INCLUDE=n8n-nodes-base.executeCommand`
   - Даже с этой настройкой может не работать

3. **Scheduler активация нестабильна**
   - Workflow требует publish:workflow для активации
   - triggerCount не всегда корректно устанавливается

**Решение**: Автономный skill-processor.js более надёжен и прозрачен.

## Troubleshooting

### Записи не обрабатываются
1. Проверить формат записи в BACKLOG.md (особенно timestamp без кавычек)
2. Запустить processor вручную и смотреть вывод
3. Проверить regex на тестовой строке

### Файлы не появляются в drafts/
1. Проверить права на директорию drafts/
2. Проверить логи continue-cli
3. Убедиться что /save endpoint работает

### Генерация некачественная
- Улучшить prompt в skill-processor.js
- Использовать более мощную модель (mistral-medium)
- Добавить примеры в prompt (few-shot)

## Output

Создаётся файл в `skills-mbb/drafts/` с именем на основе title:
- `test-skill-via-n8n-and-mistral.md`

После review человек перемещает из `drafts/` в `skills/`.

## References
- [integrations-continue-cli-mistral](../integrations/integrations-continue-cli-mistral.md)
- [integrations-n8n-local-setup](../integrations/integrations-n8n-local-setup.md)
- [BACKLOG.md](../../../BACKLOG.md)

## Metadata
- tags: #process #skills #automation #n8n #continue-cli
- dependencies: [integrations-continue-cli-mistral, integrations-n8n-local-setup]
- updated_at: 2026-01-27
- source_refs: ["skill-processor.js", "BACKLOG.md"]
