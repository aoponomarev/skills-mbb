---
title: integrations-n8n-local-setup
tags: [#integrations, #n8n, #docker]
dependencies: [skill-secrets-hygiene]
mcp_resource: true
updated_at: 2026-01-26
---

# n8n Local Setup (Docker)

## Scope
- Конфигурация локального n8n Community Edition в Docker.
- Управление данными через Named Volumes.
- Безопасное хранение секретов через .env.

## When to Use
- При необходимости развертывания или восстановления среды автоматизации (n8n) на локальной машине разработчика.
- При изменении состава секретов или путей доступа к навыкам.

## Key Rules
1. **Named Volumes Only**: Использовать n8n_data для персистентности /home/node/.n8n. Это решает проблемы с правами доступа Windows/Docker.
2. **Secrets Isolation**: Все ключи (N8N_ENCRYPTION_KEY, OAuth) хранятся в .env и никогда не попадают в git.
3. **Restart Policy**: Всегда использовать restart: unless-stopped для обеспечения автозапуска при старте Docker Desktop.
4. **File Access**: n8n должен иметь монтирование к ../skills и ../skills-mbb для работы с бэклогом.

## Workflow / Steps
1. **Подготовка**: Убедиться, что .env файл создан и содержит все ключи из secrets-backup.txt.
2. **Запуск**: Выполнить docker compose up -d.
3. **Проверка**: Открыть http://localhost:5678 и убедиться в активности workflow "Backlog Watcher".
4. **Безопасность**: Проверить git status, чтобы убедиться, что runtime-файлы n8n игнорируются.

## References
- docker-compose.yml
- .env (ignore)
- n8n/MIGRATION.md
- [Skill: skill-secrets-hygiene]

## Metadata
- tags: #integrations #n8n #automation #docker
- dependencies: [skill-secrets-hygiene]
- updated_at: 2026-01-26
- source_refs: ["docker-compose.yml", "BACKLOG.md"]
