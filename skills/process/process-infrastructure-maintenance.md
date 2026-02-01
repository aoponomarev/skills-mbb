---
id: process-infrastructure-maintenance
title: "Протокол поддержки инфраструктуры MBB"
description_ru: "Правила и процедуры обслуживания Docker-инфраструктуры, MCP серверов, автоматизации Skills и восстановления после сбоев"
scope: "Обеспечение непрерывной работоспособности всех инфраструктурных компонентов проекта MBB"
tags: [#process, #infrastructure, #devops, #maintenance, #docker, #mcp]
priority: high
created_at: 2026-01-29
updated_at: 2026-01-30
source: "mcp:infrastructure-audit"
dependencies: ["architecture-core-stack", "continue-cli-mcp-integration-nuances"]
---

# Протокол поддержки инфраструктуры MBB

> **Skill ID:** `process-infrastructure-maintenance`

## Scope

Данный навык описывает:
1. Регулярные процедуры обслуживания инфраструктуры
2. Мониторинг состояния компонентов
3. Процедуры восстановления после сбоев
4. Правила внесения изменений в инфраструктуру
5. **Процедуру миграции между рабочими станциями (Home/Office)**

## Миграция инфраструктуры (Home <-> Office)

При переносе проекта на новую рабочую станцию необходимо выполнить следующие шаги:

### 1. Синхронизация путей в конфигурации
В файле `INFRASTRUCTURE_CONFIG.yaml` необходимо проверить и обновить секцию `profiles` для текущего ПК:
- `user_profile` (обычно `%USERPROFILE%`)
- `git_path`, `node_path`, `docker_path`
- `project_root` (важно: убедиться в отсутствии лишних пробелов в путях, например `Portfolio-CV` вместо `Portfolio-CV` (исправлено))

### 2. Подготовка окружения
1. Скопировать `.env` файл из безопасного хранилища ключей.
2. Убедиться, что в `.env` прописан актуальный `CONTINUE_API_KEY`.
3. Создать Docker volume для n8n:
   ```bash
   docker volume create n8n_data
   ```

### 3. Настройка LLM (Ollama)
Если используется локальный fallback, необходимо загрузить требуемые модели:
```bash
ollama pull qwen2.5-coder:32b-instruct
ollama pull hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF:Q4_K_M
```

### 4. Верификация
После запуска контейнеров через `infra-manager.js`, запустить полную проверку:
```bash
node scripts/health-check.js
```

## Ключевые файлы

| Файл | Назначение |
|------|------------|
| `INFRASTRUCTURE_CONFIG.yaml` | Единый источник правды о конфигурации |
| `docs/INFRASTRUCTURE_RECOVERY.md` | Пошаговый план восстановления |
| `logs/issues-backlog.md` | Бэклог нерешённых проблем |
| `logs/fixes-tracking.md` | Журнал успешных исправлений |

## Критические компоненты

### 1. Docker контейнеры

**continue-cli** (КРИТИЧЕСКИЙ):
- Порт: 3002 (внешний) → 3000 (внутренний)
- Health endpoint: `http://localhost:3002/health`
- Проверка: `docker ps --filter "name=continue-cli"`

**n8n-mbb** (ОПЦИОНАЛЬНЫЙ):
- Порт: 5678
- Используется для workflow автоматизации

### 2. MCP серверы

**skills-mcp**:
- Тип: stdio (запускается Continue)
- Инструменты: `list_skills`, `read_skill`, `propose_skill`
- Логи: `/workspace/mbb/logs/mcp-debug.log`

### 3. HTTP API (continue-wrapper)

Критические эндпоинты:
- `GET /health` — проверка состояния
- `GET /ui` — Skills Management Bridge
- `GET /api/skills/*` — API навыков
- `GET /api/commits/*` — API коммитов

## Процедуры обслуживания

### Ежедневная проверка

```bash
# Быстрая проверка состояния
node scripts/health-check.js

# Ожидаемый результат:
# ✅ Overall Status: HEALTHY
# ✅ continue-wrapper [CRITICAL]
# ✅ continue-cli-container [CRITICAL]
```

### При обнаружении проблем

1. **Проверить логи контейнера**:
   ```bash
   docker logs continue-cli --tail 100
   ```

2. **Проверить логи приложения**:
   ```bash
   cat logs/skills-events.log | tail -50
   cat logs/skill-processor-errors.log | tail -20
   ```

3. **Перезапустить сервис**:
   ```bash
   docker restart continue-cli
   ```

4. **Пересобрать при необходимости**:
   ```bash
   docker compose build continue-cli --no-cache
   docker compose up -d continue-cli
   ```

### При внесении изменений в инфраструктуру

**ОБЯЗАТЕЛЬНО**:
1. Проверить `health-check.js` ДО изменений
2. Внести изменения
3. Перезапустить затронутые сервисы
4. Проверить `health-check.js` ПОСЛЕ изменений
5. Зафиксировать в `logs/fixes-tracking.md` (если это исправление)
6. Обновить `INFRASTRUCTURE_CONFIG.yaml` (если изменилась конфигурация)

## Безопасность

### Секреты

**НИКОГДА** не коммитить:
- `.env` файл
- Реальные API ключи в код
- Пароли и токены

**ВСЕГДА** использовать:
- Переменные окружения `${VARIABLE_NAME}`
- `.env.example` для документирования требуемых переменных

### Валидация входных данных

Все эндпоинты, принимающие имена файлов, ДОЛЖНЫ:
1. Использовать `path.basename()` для удаления путей
2. Фильтровать спецсимволы через regex
3. Проверять расширение файла

## Восстановление после сбоя

См. `docs/INFRASTRUCTURE_RECOVERY.md` для полного плана.

Краткий чеклист:
1. [ ] Установить Docker, Node.js, Git
2. [ ] Клонировать репозитории (skills, skills-mbb, MBB)
3. [ ] Создать `.env` из `.env.example`
4. [ ] Создать Docker volume: `docker volume create n8n_data`
5. [ ] Запустить: `docker compose up -d continue-cli`
6. [ ] Проверить: `node scripts/health-check.js`

## Мониторинг

### Автоматический

- `health-check.js` — проверка сервисов
- `status-report.js` — генерация отчёта
- `alert-manager.js` — мониторинг логов ошибок

### Ручной

- UI: http://localhost:3002/ui (вкладка Logs)
- Docker: `docker stats continue-cli`
- Логи: `docker logs -f continue-cli`

## Связанные навыки

- `integrations/continue-cli-mcp-integration-nuances.md` — нюансы Continue CLI
- `architecture/architecture-core-stack.md` — технологический стек
- `security/skill-secrets-hygiene.md` — гигиена секретов
