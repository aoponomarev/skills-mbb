---
id: process-disaster-recovery
title: Infrastructure Disaster Recovery Protocol
scope: skills-mbb
tags: [#process, #recovery, #docker, #infrastructure, #automation]
priority: high
created_at: 2026-01-31
updated_at: 2026-02-01
---

# Infrastructure Disaster Recovery Protocol

> **Critical Process**: Пошаговый план восстановления работоспособности MBB после сбоя или на чистой машине.

## Scope

Этот навык описывает процедуру восстановления среды исполнения (Docker Containers) с сохранением данных, используя OneDrive как SSOT (Single Source of Truth). Включает как автоматизированное восстановление через `infra-manager.js`, так и ручное "чистое" развертывание.

## Prerequisites

### 1. External Data (OneDrive)
Убедитесь, что папка Datasets доступна и синхронизирована:
- **Path**: `D:\Clouds\AO\OneDrive\AI\MBB\Datasets` (или аналогичный из `INFRASTRUCTURE_CONFIG.yaml`)
- **Content**:
  - `/n8n` (Database & Config)
  - `/continue` (Logs & State)

### 2. Secrets & Keys
- **MISTRAL_API_KEY**: Доступен в Password Manager.
- **CONTINUE_API_KEY**: Доступен в Password Manager.
- **N8N Encryption Key**: Доступен в Password Manager.

---

## Recovery Steps

### Phase 0: Automated Recovery (Try This First)

Для частичных сбоев (упал контейнер, завис сервис) используйте `infra-manager.js`.

1. **Check Status**:
   ```powershell
   node scripts/infra-manager.js status
   ```
2. **Attempt Auto-Recovery**:
   ```powershell
   node scripts/infra-manager.js recover
   ```
   *Скрипт попытается перезапустить упавшие сервисы, соблюдая кулдауны.*

### Phase 1: Clean Slate Preparation

Если автоматика не помогла или система повреждена, начните с очистки.

1. **Stop & Remove Containers**:
   ```powershell
   node scripts/infra-manager.js stop
   # OR manual:
   docker compose down --remove-orphans
   ```

2. **Verify Docker Desktop**:
   - Ensure WSL 2 backend is active.
   - Resource Saver mode: Auto.
   - *См. навык [WSL & Docker Optimization](./process-wsl-optimization.md) для настройки ресурсов.*

### Phase 2: Configuration & Environment

1. **Create .env**:
   ```powershell
   copy .env.example .env
   ```
2. **Populate Secrets**:
   - Заполните ключи в `.env`.
   - **ВАЖНО**: Не прописывайте ключи в `docker-compose.yml` напрямую!

3. **Verify Bind Mounts**:
   Проверьте `docker-compose.yml`, убедитесь, что пути указывают на OneDrive (через переменные или относительные пути):
   ```yaml
   volumes:
     - ${DATASETS_ROOT}/n8n:/home/node/.n8n
     - ${DATASETS_ROOT}/continue:/root/.continue
   ```

### Phase 3: Service Launch

1. **Build Custom Images** (Required for `env-subst.js`):
   ```powershell
   docker compose build continue-cli
   ```

2. **Start Services**:
   ```powershell
   node scripts/infra-manager.js start
   # OR manual:
   docker compose up -d
   ```

3. **Verify Startup**:
   ```powershell
   docker ps
   # Check logs for dynamic config generation
   docker logs continue-cli
   # Look for: "Config substitution complete"
   ```

### Phase 4: Data Verification

1. **n8n Connectivity**:
   - Open `http://localhost:5678`
   - **Check**: Ваши воркфлоу должны быть на месте (подгружены из SQLite на OneDrive).
   - *Если пусто*: Значит Docker создал новый volume вместо маунта. Проверьте путь `${DATASETS_ROOT}`.

2. **Continue CLI Health**:
   - `curl http://localhost:3002/models/status`
   - Должен вернуть JSON со статусом моделей.

---

## Troubleshooting Common Issues

### 1. `env-subst.js` Failed / API Keys Missing
**Симптом**: Continue CLI жалуется на авторизацию, или в логах "MISTRAL_API_KEY not found".
**Причина**: Скрипт подстановки переменных не отработал при старте контейнера.
**Решение**:
1. `docker compose build --no-cache continue-cli`
2. `docker compose up -d --force-recreate`

### 2. "Database is locked" (n8n)
**Симптом**: n8n не запускается, лог говорит о блокировке SQLite.
**Причина**: Файл базы данных занят другим процессом или OneDrive синхронизацией.
**Решение**:
1. Остановите n8n: `docker compose stop n8n`
2. Подождите завершения синхронизации OneDrive.
3. Запустите снова.

### 3. Missing Docker Network
**Симптом**: Контейнеры не видят друг друга.
**Решение**: `docker network inspect n8n-network`. Если нет - `docker network create n8n-network`.

## Related Skills
- [WSL & Docker Optimization](./process-wsl-optimization.md)
- [Logging Strategy](./process-logging-strategy.md)
- [Infrastructure Maintenance](./process-infrastructure-maintenance.md)
