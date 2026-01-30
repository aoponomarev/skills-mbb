---
title: integrations-n8n-docker-internals
tags: [#integrations, #n8n, #docker, #sqlite, #troubleshooting]
dependencies: [integrations-n8n-local-setup]
mcp_resource: true
updated_at: 2026-01-27
---

# n8n Docker Internals & Database Management

## Scope
- Внутренняя структура n8n в Docker-контейнере.
- Работа с SQLite базой данных n8n.
- Управление Named Volumes.
- Диагностика и восстановление.

## When to Use
- При необходимости прямого доступа к данным n8n.
- При восстановлении после сбоев.
- При миграции или резервном копировании.
- При отладке проблем с permissions.

## Key Rules

### 1. Структура данных n8n
```
/home/node/.n8n/                    # Основная директория данных
├── config                          # JSON с encryptionKey
├── database.sqlite                 # Основная БД (workflows, credentials, users)
├── database.sqlite-shm             # SQLite shared memory (runtime)
├── database.sqlite-wal             # SQLite write-ahead log (runtime)
├── crash.journal                   # Журнал сбоев
├── n8nEventLog.log                 # Текущий лог событий
├── n8nEventLog-*.log              # Ротированные логи
├── binaryData/                     # Бинарные данные workflow executions
└── nodes/                          # Кастомные ноды
```

### 2. Named Volume vs Bind Mount
```yaml
# ПРАВИЛЬНО: Named Volume (решает проблемы с правами на Windows)
volumes:
  - n8n_data:/home/node/.n8n

# НЕПРАВИЛЬНО: Bind Mount (проблемы с правами)
volumes:
  - ./n8n-data:/home/node/.n8n
```

### 3. Пользователь контейнера
- n8n работает от пользователя `node` (UID 1000, GID 1000)
- При копировании файлов через `docker cp` владелец становится root
- **Всегда** исправлять права после `docker cp`

## Важные таблицы SQLite

### user
```sql
SELECT id, email, firstName, lastName, role FROM user;
-- id: UUID пользователя (используется в API-ключах)
-- role: 'global:owner' | 'global:member'
```

### user_api_keys
```sql
SELECT id, userId, label, apiKey, createdAt, scopes FROM user_api_keys;
-- apiKey: JWT токен для API доступа
-- scopes: JSON массив разрешений
```

### workflow_entity
```sql
SELECT id, name, active, nodes, connections FROM workflow_entity;
-- active: 1 = активен, 0 = неактивен
-- nodes: JSON с конфигурацией нод
```

### credentials_entity
```sql
SELECT id, name, type, data FROM credentials_entity;
-- data: ЗАШИФРОВАННЫЕ данные (N8N_ENCRYPTION_KEY)
-- НЕЛЬЗЯ расшифровать без ключа!
```

### settings
```sql
SELECT key, value FROM settings;
-- Содержит license.cert, userManagement.isInstanceOwnerSetUp и др.
```

## Workflow: Безопасное копирование БД

### Копирование из контейнера
```bash
# 1. Остановить контейнер (важно для консистентности!)
docker stop n8n-mbb

# 2. Скопировать базу
docker cp n8n-mbb:/home/node/.n8n/database.sqlite ./backup-db.sqlite

# 3. Запустить контейнер
docker start n8n-mbb
```

### Копирование в контейнер
```bash
# 1. Остановить контейнер
docker stop n8n-mbb

# 2. Скопировать базу
docker cp ./modified-db.sqlite n8n-mbb:/home/node/.n8n/database.sqlite

# 3. Исправить права через helper container
docker run --rm -v n8n_data:/data alpine sh -c '
  chmod 666 /data/database.sqlite
  chown 1000:1000 /data/database.sqlite
  rm -f /data/database.sqlite-shm /data/database.sqlite-wal
'

# 4. Запустить контейнер
docker start n8n-mbb
```

**ВАЖНО**: Удаление `-shm` и `-wal` файлов необходимо, иначе SQLite будет пытаться использовать старые журналы.

## Workflow: Работа с SQLite через Python

```python
import sqlite3

# Подключение
conn = sqlite3.connect('backup-db.sqlite')
cursor = conn.cursor()

# Список таблиц
cursor.execute("SELECT name FROM sqlite_master WHERE type='table'")
tables = [row[0] for row in cursor.fetchall()]

# Запрос данных
cursor.execute('SELECT * FROM user_api_keys')
for row in cursor.fetchall():
    print(row)

# Обновление (с осторожностью!)
cursor.execute('UPDATE user_api_keys SET apiKey = ? WHERE userId = ?', 
               (new_token, user_id))
conn.commit()

conn.close()
```

## Диагностика проблем

### Контейнер не стартует
```bash
# Проверить логи
docker logs n8n-mbb --tail 50

# Типичные ошибки:
# - SQLITE_READONLY: проблемы с правами на database.sqlite
# - EPERM chmod: нет прав на изменение config файла
# - license cert invalid: device fingerprint изменился
```

### Контейнер отсутствует (удалён)
**Симптом**: `docker ps -a` не показывает контейнер, хотя volume `n8n_data` на месте. Браузер выдает `ERR_CONNECTION_REFUSED`.
**Причина**: Выполнение `docker compose down` или случайное удаление.
**Решение**:
```bash
docker compose up -d n8n
```
**Проверка**: `node scripts/infra-manager.js status`

### Решение SQLITE_READONLY
```bash
docker run --rm -v n8n_data:/data alpine sh -c '
  ls -la /data/
  chmod 666 /data/database.sqlite
  chown 1000:1000 /data/database.sqlite
  rm -f /data/database.sqlite-shm /data/database.sqlite-wal
'
```

### Решение проблем с config
```bash
# Проверить текущие права
docker run --rm -v n8n_data:/data alpine ls -la /data/config

# Добавить переменную для игнорирования проверки прав
environment:
  - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=false
```

### Device fingerprint mismatch (лицензия)
Лицензия n8n привязана к "fingerprint" устройства. При пересоздании контейнера:
```bash
# Удалить старую лицензию
docker exec n8n-mbb n8n license:clear

# Или через SQL (если контейнер не запускается):
sqlite3 database.sqlite "DELETE FROM settings WHERE key='license.cert'"
```

## Особенности Windows + Docker

### Git Bash преобразует пути
```bash
# ПРОБЛЕМА: Git Bash преобразует /home/node в C:/Program Files/Git/home/node
docker exec n8n-mbb cat /home/node/.n8n/config

# РЕШЕНИЕ 1: Использовать PowerShell
powershell -Command "docker exec n8n-mbb cat /home/node/.n8n/config"

# РЕШЕНИЕ 2: Экранировать путь
docker exec n8n-mbb cat //home/node/.n8n/config
```

### docker cp с пробелами в пути
```bash
# ПРОБЛЕМА: Cursor shell не понимает пути с пробелами
cd "d:\Clouds\AO\OneDrive\Portfolio-CV\..."

# РЕШЕНИЕ: Установить working_directory в параметрах Shell tool
# или использовать относительные пути внутри рабочей директории
```

## CLI команды n8n

```bash
# Справка
docker exec n8n-mbb n8n --help

# Список workflows
docker exec n8n-mbb n8n list:workflow

# Экспорт workflows
docker exec n8n-mbb n8n export:workflow --all --output=/tmp/workflows.json

# Импорт workflows
docker exec n8n-mbb n8n import:workflow --input=/tmp/workflow.json

# Очистка лицензии
docker exec n8n-mbb n8n license:clear

# Информация о лицензии
docker exec n8n-mbb n8n license:info
```

**ВАЖНО**: Для команд с путями использовать PowerShell на Windows!

## Резервное копирование

### Полный бэкап
```bash
# Остановить n8n
docker stop n8n-mbb

# Создать бэкап volume
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/n8n-backup-$(date +%Y%m%d).tar.gz -C /data .

# Запустить n8n
docker start n8n-mbb
```

### Восстановление из бэкапа
```bash
docker stop n8n-mbb

# Очистить и восстановить volume
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine sh -c '
  rm -rf /data/*
  tar xzf /backup/n8n-backup-20260127.tar.gz -C /data
  chown -R 1000:1000 /data
'

docker start n8n-mbb
```

## References
- [integrations-n8n-local-setup](./integrations-n8n-local-setup.md)
- [integrations-n8n-api-access](./integrations-n8n-api-access.md)
- [n8n CLI Commands](https://docs.n8n.io/hosting/cli-commands/)
- [Docker Named Volumes](https://docs.docker.com/storage/volumes/)

## Metadata
- tags: #integrations #n8n #docker #sqlite #troubleshooting #backup
- dependencies: [integrations-n8n-local-setup]
- updated_at: 2026-01-27
- source_refs: ["docker-compose.yml", "n8n docs"]
