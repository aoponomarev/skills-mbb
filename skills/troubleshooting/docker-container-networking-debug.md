---
title: docker-container-networking-debug
tags:
  - "#mbb-spec"
  - "#troubleshooting"
  - "#docker"
  - "#networking"
dependencies: []
mcp_resource: true
updated_at: 2026-01-28
---

# Docker Container Networking Debug: localhost vs Internal Ports

> **Контекст**: Критическая проблема при отладке межконтейнерной коммуникации в Docker, когда HTTP-клиент внутри контейнера не может подключиться к API на том же контейнере.

## Русскоязычное описание

**Паттерн отладки сетевых проблем в Docker-контейнерах**: При работе с Node.js приложениями внутри Docker-контейнеров критически важно понимать разницу между внешними (проброшенными через `ports`) и внутренними портами. Если приложение внутри контейнера пытается подключиться к API на том же контейнере, оно должно использовать **внутренний порт** (`localhost:3000`), а не внешний (`localhost:3002`). 

Эта ошибка особенно коварна, потому что:
1. С хоста (Windows/Mac) API доступен на внешнем порту (3002)
2. Из контейнера API доступен только на внутреннем порту (3000)
3. Ошибка молчаливая - нет явного сообщения о неверном порте
4. Проявляется как "timeout" или "connection refused"

Дополнительные признаки проблемы:
- HTTP-запросы с хоста работают (`curl http://localhost:3002`)
- HTTP-запросы изнутри контейнера таймаутятся
- Изменения в коде не подхватываются после рестарта (из-за Node.js require cache)

## Scope

- Docker container networking and port mapping
- HTTP client configuration inside containers
- Node.js HTTP timeout configuration
- Container debugging and log analysis

## When to Use

- При отладке межконтейнерной HTTP-коммуникации
- Когда HTTP-клиент внутри контейнера не может подключиться к API
- При молчаливых timeout-ошибках без явных сообщений
- Когда `curl` с хоста работает, а из контейнера - нет
- При необходимости настройки таймаутов для долгих LLM-запросов

## Problem Symptoms

```javascript
// WRONG: Using external port from inside container
const CONTINUE_API_URL = 'http://localhost:3002'; // ❌ External port

// Symptoms:
// - "Request timeout" after 60 seconds
// - No response from API
// - curl from host works, but not from container
```

## Solution Pattern

### 1. Correct Port Configuration

```javascript
// CORRECT: Use internal port for intra-container communication
const CONTINUE_API_URL = process.env.CONTINUE_API_URL || 'http://localhost:3000'; // ✅ Internal port
```

### 2. Adequate HTTP Timeout

```javascript
// WRONG: Too short timeout for LLM processing
req.setTimeout(60000, () => { // ❌ 60 seconds - too short
  req.destroy();
  reject(new Error('Request timeout'));
});

// CORRECT: Generous timeout for slow operations
req.setTimeout(200000, () => { // ✅ 200 seconds (3+ minutes)
  req.destroy();
  reject(new Error('Request timeout after 200s'));
});
```

### 3. Docker Compose Port Mapping

```yaml
services:
  continue-cli:
    ports:
      - "3002:3000"  # host:container
    # Internal apps use 3000
    # External access via 3002
```

## Debugging Steps

### Step 1: Test from Host

```bash
# This uses EXTERNAL port (mapped via docker-compose)
curl -X POST http://localhost:3002/prompt -H "Content-Type: application/json" -d '{"prompt":"test"}'
# ✅ Should work - proves API is running
```

### Step 2: Test from Inside Container

```bash
# This should use INTERNAL port
docker exec container-name curl -X POST http://localhost:3000/prompt -H "Content-Type: application/json" -d '{"prompt":"test"}'
# ✅ Should work if port is correct
```

### Step 3: Check Environment Variables

```bash
docker exec container-name sh -c "printenv | grep API_URL"
# Should show correct internal URL or be empty (fallback to default)
```

### Step 4: Verify File Changes

```bash
# After code changes, verify they're in container
docker exec container-name grep "localhost:3000" /path/to/file.js

# Force container recreation if changes not picked up
docker-compose down && docker-compose up -d --force-recreate service-name
```

## Why This Happens

1. **Port Mapping Confusion**: 
   - `ports: ["3002:3000"]` means: "host port 3002 → container port 3000"
   - From host: use 3002
   - From inside container: use 3000

2. **Node.js Require Cache**:
   - `docker restart` doesn't clear require cache
   - Need full container recreation: `docker-compose down && up`

3. **Volume Mount Delays**:
   - Sometimes file changes from host take time to propagate
   - Use `--force-recreate` to ensure fresh state

## Related Issues

- **Timeout Mismatches**: If API wrapper has 180s timeout, HTTP client needs 200s+
- **Silent Failures**: Node.js HTTP client doesn't always throw clear errors
- **Log Buffering**: Docker logs may show old output; use `--since 1m` for fresh logs

## Red Flags for AI Agents

When you see these patterns, suspect networking/port issues:

```
❌ "Sending prompt to Continue API..."
❌ "⏭️ Skipped due to LLM error"
❌ (no "API response" log between them)

✅ Expected flow:
✅ "Sending prompt to Continue API..."
✅ "[Debug] API response status: 200, body length: 1234"
✅ "[LLM] Generated explanation via Continue Mistral"
```

## Testing Checklist

- [ ] Test HTTP call from host (proves API works)
- [ ] Test HTTP call from inside container (proves networking)
- [ ] Verify environment variables in container
- [ ] Check file content in container after changes
- [ ] Use `--force-recreate` to eliminate cache issues
- [ ] Monitor logs in real-time during test: `docker logs -f container-name`
- [ ] Ensure timeouts are sufficient for slowest operation (LLM: 60-180s typical)

## Key Takeaways

1. **localhost** inside container ≠ **localhost** on host
2. Use **internal ports** for intra-container communication
3. Use **external ports** only from host machine
4. Set HTTP timeouts **higher** than the longest expected operation
5. Use `docker-compose down && up --force-recreate` to ensure clean state
6. Always verify changes propagated to container before testing

## Example: Real-World Debugging Session

```bash
# 1. Problem: skill-watcher.js timing out when calling Continue API
# Symptom: "Sending prompt..." → immediate "Skipped due to LLM error"

# 2. Test from host (proves API works)
$ curl http://localhost:3002/prompt -d '{"prompt":"test"}'
# ✅ Returns {"success":true,...} in 15 seconds

# 3. Check what URL skill-watcher uses
$ docker exec continue-cli grep CONTINUE_API_URL /workspace/mbb/scripts/skill-watcher.js
# ❌ Found: http://localhost:3002

# 4. Fix: Change to internal port
const CONTINUE_API_URL = 'http://localhost:3000'; // ✅

# 5. Recreate container (clear cache)
$ docker-compose down && docker-compose up -d continue-cli

# 6. Test scan
$ curl -X POST http://localhost:3002/api/skills/scan

# 7. Monitor logs
$ docker logs -f continue-cli
# ✅ Now shows: "[LLM] Generated explanation via Continue Mistral"
```

## Conclusion

Эта проблема - классический пример того, как архитектурное решение (проброс портов в Docker) создаёт неинтуитивное поведение. AI-агентам критически важно понимать:
- Откуда выполняется HTTP-запрос (хост или контейнер)
- Какой порт использовать в каждом контексте
- Как правильно отлаживать межконтейнерную коммуникацию
- Необходимость force-recreate для применения изменений в коде

Без этого понимания отладка может затянуться на часы, с ним - решается за минуты.
