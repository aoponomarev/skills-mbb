---
id: troubleshoot-continue-cli-api-keys
title: Troubleshooting Continue CLI API Key Substitution in Docker
scope: skills-mbb
tags: [continue-cli, docker, api-keys, troubleshooting, env-vars]
priority: high
created: 2026-01-31
updated: 2026-01-31
---

# Troubleshooting Continue CLI API Key Substitution in Docker

## Problem

When updating LLM API keys (Mistral, Groq, OpenRouter) in `.env` file, the Continue CLI container may continue using **old keys** even after restart, causing `401 Unauthorized` errors.

## Root Causes

### 1. Docker Compose Environment Variable Precedence

```yaml
# BAD: Explicit env vars override env_file
env_file:
  - .env
environment:
  - MISTRAL_API_KEY=${MISTRAL_API_KEY}  # ← Interpolated from SHELL, not .env!
```

The `${VAR}` syntax in `environment:` section is interpolated from the **host shell environment**, not from the `.env` file. If the shell has an old cached value, it will be used.

### 2. Config.yaml Variable Syntax Not Interpreted

Continue CLI's `config.yaml` uses `${VAR_NAME}` syntax, but **does not automatically substitute** environment variables at runtime:

```yaml
# This does NOT work automatically:
apiKey: ${MISTRAL_API_KEY}
```

### 3. Docker Container Caching

Even with `docker compose up -d --force-recreate`, environment variables may be cached in the container layer.

## Solutions

### Solution 1: Remove Duplicate Environment Variables

Remove explicit `environment:` entries that duplicate `env_file`:

```yaml
# GOOD: Only use env_file
env_file:
  - .env
environment:
  - CONTINUE_WRAPPER_PORT=3000  # Only non-secret vars here
```

### Solution 2: Create env-subst.js Script

Create a Node.js script to substitute environment variables in config.yaml at container startup:

**File: `mcp/continue-wrapper/env-subst.js`**

```javascript
#!/usr/bin/env node
const fs = require('fs');

const templatePath = '/workspace/.continue-template/config.yaml';
const outputPath = '/root/.continue/config.yaml';

let content = fs.readFileSync(templatePath, 'utf8');

// Substitute all ${VAR_NAME} patterns with environment variables
content = content.replace(/\$\{([A-Z_]+)\}/g, (match, varName) => {
  return process.env[varName] || match;
});

fs.writeFileSync(outputPath, content);
console.log('Config substitution complete');
```

### Solution 3: Update Docker Compose Command

Add the substitution script to container startup:

```yaml
command: >
  sh -c "echo 'PREPARING RUNTIME CONFIG...' &&
  mkdir -p /root/.continue &&
  cp -r /workspace/.continue-template/* /root/.continue/ &&
  echo 'Substituting env vars in config...' &&
  node /workspace/mbb/mcp/continue-wrapper/env-subst.js &&
  echo 'Starting wrapper...' &&
  node /workspace/mbb/mcp/continue-wrapper/server.js"
```

### Solution 4: Full Container Recreation

When updating API keys, perform a **full recreation**:

```bash
# 1. Update .env file with new key
# 2. Stop and remove container completely
docker stop continue-cli && docker rm continue-cli

# 3. Recreate with fresh env
docker compose up -d continue-cli

# 4. Verify new key is loaded
docker exec continue-cli printenv MISTRAL_API_KEY | head -c 10
```

## Verification Steps

### Step 1: Check .env File on Host

```bash
grep MISTRAL_API_KEY .env
# Should show: MISTRAL_API_KEY=<new-key>
```

### Step 2: Check Environment in Container

```bash
docker exec continue-cli printenv MISTRAL_API_KEY
# Should match the new key from .env
```

### Step 3: Check Substituted Config

```bash
docker exec continue-cli grep apiKey /root/.continue/config.yaml | head -3
# Should show actual key values, NOT ${VAR_NAME}
```

### Step 4: Test API Call

```bash
docker exec continue-cli cn --auto --config /root/.continue/config.yaml -p "Say hello"
# Should return response, not 401 error
```

## Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `401 status code (no body)` | Old/invalid API key | Update .env, recreate container |
| `apiKey: ${MISTRAL_API_KEY}` in config | env-subst.js not running | Check docker-compose command |
| Old key after restart | Shell env cached | Remove explicit `environment:` entries |
| `envsubst: not found` | Alpine image lacks gettext | Use Node.js env-subst.js instead |

## Model Fallback Chain

To prevent service disruption when one provider fails, implement automatic fallback:

```javascript
const MODEL_FALLBACK_CHAIN = [
  { name: 'mistral-small', provider: 'mistral' },
  { name: 'groq-llama-3.3-70b', provider: 'groq' },
  { name: 'groq-qwen-32b', provider: 'groq' },
  { name: 'or-deepseek-chat', provider: 'openrouter' },
  { name: 'or-deepseek-r1', provider: 'openrouter' },
  { name: 'or-llama-3.3-70b', provider: 'openrouter' },
];
```

Monitor model health via: `GET http://localhost:3002/models/status`

## Related Files

- `.env` — API keys storage (gitignored)
- `.continue/config.yaml` — Template with `${VAR}` placeholders
- `mcp/continue-wrapper/env-subst.js` — Substitution script
- `mcp/continue-wrapper/server.js` — Wrapper with fallback logic
- `docker-compose.yml` — Container configuration

## See Also

- [process-n8n-docker-code-nodes.md](./process-n8n-docker-code-nodes.md) — n8n Code node configuration
- [process-n8n-browser-cache.md](./process-n8n-browser-cache.md) — Browser cache issues
