---
id: process-n8n-docker-code-nodes
title: n8n Docker - Code Nodes и Execute Command
category: process
status: active
created: 2026-01-31
updated: 2026-01-31
tags: [n8n, docker, code-nodes, child_process, troubleshooting]
---

# n8n Docker: Code Nodes и Execute Command

## Проблема

При запуске n8n в Docker возникают ошибки:
- `Unrecognized node type: n8n-nodes-base.executeCommand`
- `Module 'child_process' is disallowed`
- `Module 'fs' is disallowed`
- `fetch is not defined`

## Причина

n8n по умолчанию блокирует опасные модули Node.js в Code-нодах для безопасности. Также `Execute Command` может быть недоступен в некоторых конфигурациях.

## Решение

### 1. Настройка docker-compose.yml

```yaml
services:
  n8n:
    environment:
      # Разрешить встроенные модули Node.js в Code-нодах
      - NODE_FUNCTION_ALLOW_BUILTIN=child_process,fs,path,http,https,crypto,os,url,util
      # Разрешить внешние модули (или конкретный список)
      - NODE_FUNCTION_ALLOW_EXTERNAL=*
      # ВАЖНО: отключить runners для работы в основном процессе
      - N8N_RUNNERS_ENABLED=false
      # Разрешить доступ к env-переменным
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=false
```

### 2. Альтернатива Execute Command — Code Node

Вместо `n8n-nodes-base.executeCommand` использовать `n8n-nodes-base.code`:

```javascript
const { execSync } = require('child_process');

const stdout = execSync('node /path/to/script.js', {
  encoding: 'utf8',
  timeout: 120000
});

return [{ json: { stdout } }];
```

### 3. Для записи в файлы

```javascript
const fs = require('fs');

const data = $input.first().json.data ?? '';
const timestamp = new Date().toISOString();
const entry = `[${timestamp}] ${data}\n`;

fs.appendFileSync('/home/node/mbb/logs/output.log', entry, 'utf8');

return $input.all();
```

## Важные замечания

1. **N8N_RUNNERS_ENABLED=false** — критически важно! Без этого код выполняется в изолированном runner-процессе, где модули недоступны.

2. **Sticky Notes не интерпретируют выражения** — `{{ $json.field }}` отображается как текст, а не значение.

3. **После изменения env-переменных** — обязательно `docker compose up -d --force-recreate n8n`.

## Дополнительно: замена Ollama на Continue CLI

Если воркфлоу использует `n8n-nodes-base.ollamaChatModel` (community node), лучше заменить на HTTP-запрос к Continue CLI:

```javascript
// Вместо Ollama node — HTTP Request к Continue CLI
{
  "method": "POST",
  "url": "http://continue-cli:3000/prompt",
  "jsonBody": "={{ JSON.stringify({ prompt: $json.prompt }) }}",
  "options": { "timeout": 120000 }
}
```

**Преимущества Continue CLI:**
- Облачные модели (Mistral, Groq, OpenRouter) — в 10 раз быстрее локальных
- Не требует установки community nodes
- Уже настроен в Docker-сети

## Связанные навыки

- [[process-n8n-browser-cache]]
- [[docker-compose-n8n-config]]
