---
title: integrations-n8n-api-access
tags: [#integrations, #n8n, #api, #docker, #jwt, #security]
dependencies: [integrations-n8n-local-setup, skill-secrets-hygiene]
mcp_resource: true
updated_at: 2026-01-27
---

# n8n API Access & JWT Key Generation

## Scope
- Программный доступ к n8n через Public API.
- Генерация и валидация JWT API-ключей.
- Диагностика и решение проблем с аутентификацией.
- Работа с n8n базой данных через Docker.

## When to Use
- При необходимости программного управления workflows (активация, деактивация, запуск).
- При ошибках "invalid signature" или "unauthorized" в API-запросах.
- При смене `N8N_ENCRYPTION_KEY` или восстановлении среды.
- При автоматизации через внешние системы (Cursor Agent, CI/CD).

## Key Rules

### 1. JWT Secret Derivation
n8n **НЕ использует** переменную `JWT_SECRET` из окружения для подписи API-ключей. Вместо этого:

```javascript
// Алгоритм генерации JWT secret из N8N_ENCRYPTION_KEY
const crypto = require('crypto');
const encryptionKey = process.env.N8N_ENCRYPTION_KEY; // например: 'mbb-secret-key-2026'

// Взять каждую ВТОРУЮ букву (индексы 0, 2, 4, ...)
let baseKey = '';
for (let i = 0; i < encryptionKey.length; i += 2) {
  baseKey += encryptionKey[i];
}
// baseKey = 'mbsce-e-06'

// Хэшировать SHA256
const jwtSecret = crypto.createHash('sha256').update(baseKey).digest('hex');
// jwtSecret = '400f5318eedc10cf4965e4cd8d7801e869ba7d45718bd49983ebd30bd982eb38'
```

**ВАЖНО**: Если изменить `N8N_ENCRYPTION_KEY`, все существующие API-ключи станут недействительными!

### 2. Структура API-ключа
```javascript
const payload = {
  sub: '<user-uuid>',      // UUID пользователя из таблицы user
  iss: 'n8n',              // Издатель (фиксировано)
  aud: 'public-api',       // Аудитория (фиксировано)
  iat: <unix-timestamp>    // Время создания
};
```

### 3. Хранение ключей
- API-ключи хранятся в таблице `user_api_keys` SQLite базы.
- Поле `apiKey` содержит полный JWT токен.
- При обновлении ключа нужно обновить и токен в базе, и локальный `.env`.

### 4. Права доступа к файлам
При копировании файлов в Docker volume права могут сбиться:
```bash
# Исправление прав через helper container
docker run --rm -v n8n_data:/data alpine sh -c \
  'chmod 666 /data/database.sqlite; chown 1000:1000 /data/database.sqlite'
```

## Workflow: Генерация нового API-ключа

### Шаг 1: Вычислить JWT Secret
```bash
node -e "
const crypto = require('crypto');
const encryptionKey = 'YOUR_ENCRYPTION_KEY';
let baseKey = '';
for (let i = 0; i < encryptionKey.length; i += 2) {
  baseKey += encryptionKey[i];
}
const jwtSecret = crypto.createHash('sha256').update(baseKey).digest('hex');
console.log('JWT Secret:', jwtSecret);
"
```

### Шаг 2: Получить UUID пользователя
```bash
# Через Python (sqlite3 встроен)
docker cp n8n-mbb:/home/node/.n8n/database.sqlite ./temp-db.sqlite
python -c "
import sqlite3
conn = sqlite3.connect('temp-db.sqlite')
cursor = conn.cursor()
cursor.execute('SELECT id, email FROM user')
for row in cursor.fetchall():
    print(f'UUID: {row[0]}, Email: {row[1]}')
"
```

### Шаг 3: Сгенерировать JWT токен
```bash
# Требуется: npm install jsonwebtoken
node -e "
const jwt = require('jsonwebtoken');
const payload = {
  sub: 'USER_UUID_HERE',
  iss: 'n8n',
  aud: 'public-api',
  iat: Math.floor(Date.now() / 1000)
};
const secret = 'COMPUTED_JWT_SECRET_HERE';
console.log(jwt.sign(payload, secret));
"
```

### Шаг 4: Обновить токен в базе данных
```bash
docker stop n8n-mbb

python -c "
import sqlite3
conn = sqlite3.connect('temp-db.sqlite')
cursor = conn.cursor()
new_token = 'NEW_JWT_TOKEN_HERE'
user_id = 'USER_UUID_HERE'
cursor.execute('UPDATE user_api_keys SET apiKey = ? WHERE userId = ?', (new_token, user_id))
conn.commit()
print(f'Updated {cursor.rowcount} rows')
"

docker cp temp-db.sqlite n8n-mbb:/home/node/.n8n/database.sqlite

# Исправить права
docker run --rm -v n8n_data:/data alpine sh -c \
  'chmod 666 /data/database.sqlite; chown 1000:1000 /data/database.sqlite; rm -f /data/database.sqlite-shm /data/database.sqlite-wal'

docker start n8n-mbb
```

### Шаг 5: Проверить доступ
```bash
curl -s -X GET "http://localhost:5678/api/v1/workflows" \
  -H "X-N8N-API-KEY: YOUR_NEW_TOKEN" | head -100
```

## Диагностика ошибок

### "invalid signature"
**Причина**: JWT подписан неправильным секретом.
**Решение**: Пересчитать JWT secret из актуального `N8N_ENCRYPTION_KEY`.

### "unauthorized"
**Причина**: Подпись верна, но токен не найден в базе данных.
**Решение**: Обновить токен в таблице `user_api_keys`.

### "SQLITE_READONLY"
**Причина**: Неправильные права на database.sqlite после docker cp.
**Решение**: Исправить права через alpine helper container (см. выше).

### Ошибки с путями в Git Bash на Windows
Git Bash на Windows преобразует Unix-пути в Windows-пути:
```bash
# НЕПРАВИЛЬНО (Git Bash преобразует путь):
docker exec n8n-mbb chmod 666 /home/node/.n8n/database.sqlite
# Результат: chmod: C:/Program Files/Git/home/node/...

# ПРАВИЛЬНО (через PowerShell):
powershell -Command "docker exec n8n-mbb chmod 666 /home/node/.n8n/database.sqlite"
```

## API Endpoints Reference

```bash
# Список workflows
curl -H "X-N8N-API-KEY: $TOKEN" http://localhost:5678/api/v1/workflows

# Активировать workflow
curl -X PATCH -H "X-N8N-API-KEY: $TOKEN" \
  http://localhost:5678/api/v1/workflows/{id}/activate

# Деактивировать workflow
curl -X PATCH -H "X-N8N-API-KEY: $TOKEN" \
  http://localhost:5678/api/v1/workflows/{id}/deactivate

# Запустить workflow вручную
curl -X POST -H "X-N8N-API-KEY: $TOKEN" \
  http://localhost:5678/api/v1/workflows/{id}/run
```

## Security Checklist
- [ ] API-ключ хранится ТОЛЬКО в `.env` (защищён `.gitignore`)
- [ ] Временные файлы БД удалены после операций
- [ ] `N8N_ENCRYPTION_KEY` не коммитится в git
- [ ] При смене encryption key — пересоздать все API-ключи

## References
- [integrations-n8n-local-setup](./integrations-n8n-local-setup.md)
- [skill-secrets-hygiene](../../skills/security/skill-secrets-hygiene.md)
- [n8n JWT Service Source](https://fossies.org/linux/n8n/packages/cli/src/services/jwt.service.ts)
- [n8n API Docs](https://docs.n8n.io/api/)

## Metadata
- tags: #integrations #n8n #api #jwt #docker #security
- dependencies: [integrations-n8n-local-setup, skill-secrets-hygiene]
- updated_at: 2026-01-27
- source_refs: ["docker-compose.yml", ".env", "n8n JWT Service Source"]
