---
id: integrations-continue-mcp-setup
title: Continue CLI & MCP Integration Setup
scope: skills-mbb
tags: [#integration, #continue, #mcp, #ai]
priority: high
created_at: 2026-01-31
updated_at: 2026-01-31
---

# Continue CLI & MCP Integration Setup

> **Integration Protocol**: Обеспечивает связь между Continue AI Agent и базой знаний проекта (Skills) через протокол MCP (Model Context Protocol).

## Architecture

```
Continue CLI Agent (Docker)
    ↓ (MCP Protocol via Stdio)
skills-mcp server (Node.js)
    ↓ (File System Access)
├── skills/skills/        (General Skills)
└── skills-mbb/skills/    (Project Skills)
```

## Configuration

### 1. Continue Config (`config.yaml`)

Конфигурация находится в `.continue/config.yaml` и динамически обновляется при старте контейнера (скрипт `env-subst.js` подставляет API ключи).

#### MCP Server Definition
```yaml
mcpServers:
  - name: skills-mcp
    command: node
    args:
      - /workspace/mbb/mcp/skills-mcp/server.js
    env:
      # Наследует PATH и переменные контейнера
```

#### System Message (Mandatory Skills Usage)
Мы принуждаем агента использовать скиллы *до* написания кода.

```yaml
systemMessage: |
  You are an AI coding assistant working on the MBB project.
  ...
  MANDATORY WORKFLOW:
  1. Before starting ANY task, use list_skills() to find relevant skills
  2. Read applicable skills with read_skill()
  3. If encountering new patterns, propose_skill()
```

### 2. Available MCP Tools

Инструменты доступны агенту автоматически.

#### `list_skills(query, tags)`
Поиск навыков.
```javascript
// Find Docker skills
list_skills({query: "docker"})
```

#### `read_skill(id, repo)`
Чтение полного текста навыка.
```javascript
read_skill({id: "process-disaster-recovery", repo: "skills-mbb"})
```

#### `propose_skill(title, scope...)`
Создание черновика нового навыка в `skills-mbb/drafts/`.

## Verification

### Check MCP Server Status
1. Зайти в контейнер:
   ```bash
   docker exec -it continue-cli sh
   ```
2. Проверить процесс:
   ```bash
   ps aux | grep server.js
   ```

### Debug Logs
Логи MCP сервера пишутся в `logs/mcp-debug.log` (через перенаправление в `server.js` или Docker logs).

## Troubleshooting

| Problem | Solution |
| :--- | :--- |
| **"mcpServers not found"** | Verify `.continue/config.yaml` syntax. Restart container. |
| **Skills not visible** | Check Docker volume mounts for `/workspace/skills`. |
| **API Key Error** | See [Troubleshoot API Keys](../troubleshooting/troubleshoot-continue-cli-api-keys.md). |

## Related Skills
- [A_AI_ORCHESTRATION (High Level Architecture)](../../docs/A_AI_ORCHESTRATION.md)
- [Troubleshoot Continue API Keys](../troubleshooting/troubleshoot-continue-cli-api-keys.md)
- [Disaster Recovery](./process-disaster-recovery.md)
