---
title: integrations-n8n-local-setup
tags: [#integrations, #n8n, #docker]
dependencies: [skill-secrets-hygiene]
mcp_resource: true
updated_at: 2026-01-27
---

# n8n Local Setup (Docker)

## Scope
- Конфигурация локального n8n Community Edition в Docker.
- Управление данными через Named Volumes.
- Безопасное хранение секретов через .env.

**Не покрывает** (см. связанные скиллы):
- Программный доступ через API → [integrations-n8n-api-access](./integrations-n8n-api-access.md)
- Работа с внутренней БД → [integrations-n8n-docker-internals](./integrations-n8n-docker-internals.md)

## When to Use
- При необходимости развертывания или восстановления среды автоматизации (n8n) на локальной машине разработчика.
- При изменении состава секретов или путей доступа к навыкам.

## Key Rules
1. **Named Volumes Only**: Использовать n8n_data для персистентности /home/node/.n8n. Это решает проблемы с правами доступа Windows/Docker.
2. **Secrets Isolation**: Все ключи (N8N_ENCRYPTION_KEY, OAuth, N8N_API_KEY) хранятся в .env и никогда не попадают в git.
3. **Restart Policy**: Всегда использовать restart: unless-stopped для обеспечения автозапуска при старте Docker Desktop.
4. **File Access**: n8n должен иметь монтирование к ../skills и ../skills-mbb для работы с бэклогом.
5. **Encryption Key**: `N8N_ENCRYPTION_KEY` критически важен — от него зависят расшифровка credentials и генерация JWT secret для API.

## Workflow / Steps
1. **Подготовка**: Убедиться, что .env файл создан и содержит все ключи.
2. **Создать Named Volume** (если первый запуск):
   ```bash
   docker volume create n8n_data
   ```
3. **Запуск**: Выполнить `docker compose up -d`.
4. **Проверка UI**: Открыть http://localhost:5678 и убедиться в активности workflow "Backlog Watcher".
5. **Проверка API** (опционально):
   ```bash
   curl -H "X-N8N-API-KEY: $N8N_API_KEY" http://localhost:5678/api/v1/workflows
   ```
6. **Безопасность**: Проверить `git status`, чтобы убедиться, что runtime-файлы n8n игнорируются.

## Environment Variables
```bash
# Обязательные
N8N_ENCRYPTION_KEY=your-encryption-key  # Критически важен!

# API доступ
N8N_API_KEY=eyJ...                      # JWT токен для программного доступа

# OAuth (для Google интеграций)
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...

# Cloudflare (если используется)
CLOUDFLARE_ACCOUNT_ID=...
CLOUDFLARE_API_TOKEN=...
```

## References
- docker-compose.yml
- .env (защищён .gitignore)
- n8n/MIGRATION.md
- [integrations-n8n-api-access](./integrations-n8n-api-access.md) — программный доступ через API
- [integrations-n8n-docker-internals](./integrations-n8n-docker-internals.md) — внутренняя структура и БД
- [skill-secrets-hygiene](../../skills/security/skill-secrets-hygiene.md)

## Metadata
- tags: #integrations #n8n #automation #docker
- dependencies: [skill-secrets-hygiene]
- updated_at: 2026-01-27
- source_refs: ["docker-compose.yml", "BACKLOG.md"]
