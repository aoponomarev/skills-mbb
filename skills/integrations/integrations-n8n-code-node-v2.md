---
title: integrations-n8n-code-node-v2
tags: [#integrations, #n8n, #javascript, #troubleshooting]
dependencies: [integrations-n8n-local-setup]
mcp_resource: true
updated_at: 2026-01-27
---

# n8n Code Node v2: Gotchas & Patterns

## Scope
- Особенности Code node в n8n 2.x.
- Различия между версиями API.
- Рабочие паттерны и антипаттерны.

## When to Use
- При написании JavaScript кода в n8n workflows.
- При отладке Code nodes, которые не работают.
- При миграции workflows с n8n 1.x на 2.x.

## Key Changes in n8n 2.x

### API изменения

| Feature | n8n 1.x | n8n 2.x |
|---------|---------|---------|
| Input items | `items` | `$input.all()` или `items` |
| First item | `items[0]` | `$input.first()` |
| Node output | `$node['Name'].json` | Аналогично, но с нюансами |
| Context | `$env`, `$json` | Расширенный контекст |

### Проблема: Code node не получает данные

**Симптом**: Workflow выполняется успешно, но Code node возвращает пустой массив или undefined.

**Причина**: В n8n 2.x Code node работает в Task Runner (отдельном процессе), что влияет на доступ к данным.

**Решение**: Использовать явный доступ через `$input`:

```javascript
// РАБОТАЕТ в n8n 2.x
const items = $input.all();
const firstItem = $input.first();

// МОЖЕТ НЕ РАБОТАТЬ
const items = items;  // Конфликт имён
const item = items[0]; // Может быть undefined
```

## Рабочие паттерны

### Чтение binary файла
```javascript
const item = $input.first();
if (!item.binary || !item.binary.data) {
  return [{ json: { error: 'No binary data' } }];
}

const binaryData = item.binary.data;
const content = Buffer.from(binaryData.data, 'base64').toString('utf8');
```

### Возврат нескольких items
```javascript
const results = [];
for (const match of matches) {
  results.push({
    json: {
      title: match[1],
      scope: match[2]
    }
  });
}
return results;
```

### Пустой результат (skip downstream)
```javascript
// Возврат пустого массива пропускает downstream nodes
if (noData) {
  return [];
}
```

### Доступ к данным из других nodes
```javascript
// Получить данные из node 'Build Prompt'
const title = $node['Build Prompt'].json.title;
const filename = $node['Build Prompt'].json.filename;
```

## Антипаттерны

### Не работает: ES6 в старом синтаксисе
```javascript
// ПЛОХО: Может не работать
const item = $input.first();
const { binary } = item;

// ЛУЧШЕ: Явный доступ
const item = $input.first();
const binary = item.binary;
```

### Не работает: console.log для отладки
```javascript
// console.log НЕ виден в логах n8n!
// Используйте возврат данных для отладки:
return [{ json: { debug: 'value', data: content.length } }];
```

### Не работает: Async/await в некоторых случаях
```javascript
// Если async не работает, используйте callbacks или синхронный код
// Или проверьте что node настроен как async
```

## executeCommand Node

В n8n 2.0+ executeCommand отключен по умолчанию.

### Включение
```yaml
# docker-compose.yml
environment:
  - N8N_NODES_INCLUDE=n8n-nodes-base.executeCommand
  - N8N_BLOCK_EXECUTE_COMMAND=false
```

### Альтернатива
Вместо executeCommand используйте HTTP Request к внешнему сервису:

```javascript
// Вместо: cat /path/to/file
// Используйте: HTTP Request к API endpoint

// В continue-wrapper добавлен /save endpoint
// который записывает файлы через HTTP
```

## Workflow Activation Issues

### Проблема: Workflow активен в БД, но не запускается

1. **publish:workflow**
   ```bash
   docker exec n8n n8n publish:workflow --id=WORKFLOW_ID
   ```

2. **triggerCount**
   ```sql
   UPDATE workflow_entity SET triggerCount = 1 WHERE id = 'WORKFLOW_ID';
   ```

3. **Рестарт n8n после изменений в БД**
   ```bash
   docker restart n8n-mbb
   ```

### Проблема: Scheduler trigger не срабатывает

- Проверить что workflow активирован в логах при старте n8n
- Время в контейнере может быть UTC (не локальное!)
- Проверить executions в БД:
  ```sql
  SELECT * FROM execution_entity WHERE workflowId = 'ID' ORDER BY startedAt DESC;
  ```

## Рекомендация

Для сложной логики обработки файлов лучше использовать **внешний скрипт** вместо n8n Code node:
- Более прозрачная отладка
- Полный контроль над выполнением
- Независимость от версии n8n

См. [process-skill-pipeline](../process/process-skill-pipeline.md)

## References
- [n8n Code Node Docs](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.code/)
- [integrations-n8n-local-setup](./integrations-n8n-local-setup.md)
- [integrations-n8n-docker-internals](./integrations-n8n-docker-internals.md)

## Metadata
- tags: #integrations #n8n #javascript #troubleshooting #code-node
- dependencies: [integrations-n8n-local-setup]
- updated_at: 2026-01-27
- source_refs: ["n8n docs", "experimental findings"]
