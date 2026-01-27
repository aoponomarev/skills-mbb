---
title: integrations-continue-cli-mistral
tags: [#integrations, #continue-cli, #mistral, #llm, #automation]
dependencies: [integrations-n8n-local-setup]
mcp_resource: true
updated_at: 2026-01-27
---

# Continue CLI + Mistral Integration

## Scope
- Настройка Continue CLI в headless режиме для автоматизации.
- Интеграция с Mistral API как основной LLM.
- Ollama как fallback.
- HTTP wrapper для интеграции с n8n и другими системами.

## When to Use
- При автоматизации генерации контента через LLM.
- При построении пайплайнов обработки текста.
- Для skill-processor и подобных автоматических систем.

## Architecture

```
┌─────────────────┐     ┌─────────────────────┐     ┌─────────────┐
│  n8n / cron     │────▶│  continue-wrapper   │────▶│ Continue CLI│
│  (trigger)      │     │  (HTTP API)         │     │ (headless)  │
└─────────────────┘     └─────────────────────┘     └──────┬──────┘
                                                          │
                                    ┌─────────────────────┼─────────────────────┐
                                    ▼                     ▼                     ▼
                             ┌──────────┐          ┌──────────┐          ┌──────────┐
                             │ Mistral  │          │  Ollama  │          │  Other   │
                             │  (API)   │          │ (local)  │          │   LLMs   │
                             └──────────┘          └──────────┘          └──────────┘
```

## Key Components

### 1. Continue CLI Config (.continue/config.yaml)
```yaml
models:
  - name: Mistral Small
    provider: mistral
    model: mistral-small-latest
    apiKey: ${MISTRAL_API_KEY}  # Патчится при запуске
    roles: [chat]
    
  - name: Ollama Qwen (Fallback)
    provider: ollama
    model: qwen2.5-coder:32b-instruct
    apiBase: http://host.docker.internal:11434
    roles: [chat]
```

### 2. HTTP Wrapper (server.js)
```javascript
// Endpoints:
// GET  /health  - Health check
// POST /prompt  - Execute CLI with {"prompt": "..."}
// POST /save    - Save file with {"filepath": "...", "content": "..."}

async function runContinueCLI(prompt) {
  const cmd = `/usr/local/bin/cn -p "${escapedPrompt}"`;
  return await execAsync(cmd, { timeout: TIMEOUT });
}
```

### 3. Docker Service
```yaml
continue-cli:
  build:
    context: .
    dockerfile: docker/continue-cli/Dockerfile
  volumes:
    - ./.continue:/workspace/.continue-template:ro
  environment:
    - MISTRAL_API_KEY=${MISTRAL_API_KEY}
  command: >
    sh -c "mkdir -p /root/.continue && 
    cp -r /workspace/.continue-template/* /root/.continue/ && 
    sed -i 's/\$${MISTRAL_API_KEY}/'$$MISTRAL_API_KEY'/g' /root/.continue/config.yaml && 
    node /workspace/mbb/mcp/continue-wrapper/server.js"
```

## Runtime API Key Patching

**Проблема**: API ключи не должны храниться в git, но Continue CLI читает config.yaml напрямую.

**Решение**: Runtime patching при запуске контейнера:
1. Config хранится с placeholder `${MISTRAL_API_KEY}`
2. При старте контейнера sed заменяет placeholder на реальное значение из env
3. Оригинальный файл монтируется как read-only template

```bash
# Патч API ключа
sed -i 's/\${MISTRAL_API_KEY}/'$MISTRAL_API_KEY'/g' /root/.continue/config.yaml
```

## Usage Examples

### Вызов через curl
```bash
# Health check
curl http://localhost:3001/health

# Generate content
curl -X POST http://localhost:3001/prompt \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Generate a skill document for..."}'

# Save file
curl -X POST http://localhost:3001/save \
  -H "Content-Type: application/json" \
  -d '{"filepath": "skill.md", "content": "# Skill\n..."}'
```

### Внутри Docker network
```javascript
// n8n HTTP Request node
const result = await fetch('http://continue-cli:3000/prompt', {
  method: 'POST',
  body: JSON.stringify({ prompt: myPrompt })
});
```

## Troubleshooting

### CLI не отвечает
```bash
# Проверить логи
docker logs continue-cli --tail 50

# Проверить health
curl http://localhost:3001/health

# Проверить config внутри контейнера
docker exec continue-cli cat /root/.continue/config.yaml
```

### Mistral API ошибки
- Проверить баланс на console.mistral.ai
- Проверить что MISTRAL_API_KEY в .env корректен
- Проверить что sed правильно заменил placeholder

### Timeout при генерации
- Увеличить CONTINUE_TIMEOUT env variable
- Проверить сетевую доступность Mistral API

## References
- [Continue CLI Docs](https://continue.dev/docs)
- [Mistral API](https://docs.mistral.ai/)
- [integrations-n8n-local-setup](./integrations-n8n-local-setup.md)

## Metadata
- tags: #integrations #continue-cli #mistral #llm #automation
- dependencies: [integrations-n8n-local-setup]
- updated_at: 2026-01-27
- source_refs: ["mcp/continue-wrapper/server.js", ".continue/config.yaml"]
