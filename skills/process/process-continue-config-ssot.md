---
id: process-continue-config-ssot
title: "Continue Config SSOT (OneDrive)"
description_ru: "Единый источник правды для конфигурации Continue на Home/Office. Путь, переменные окружения, junction и формат config.yaml."
scope: "Настройка и контроль Continue-конфига для Windows (Cursor/Continue) с OneDrive как SSOT."
tags: [#process, #continue, #config, #ssot, #windows, #onedrive]
priority: high
created_at: 2026-01-30
updated_at: 2026-01-30
dependencies: ["process-infrastructure-maintenance", "protocol-agent-core", "security/skill-secrets-hygiene"]
---

# Continue Config SSOT (OneDrive)

## Scope
- Единая точка хранения конфигурации Continue для Home/Office.
- Правильное подключение через `CONTINUE_HOME`, `continue.continuePath` и junction.
- Валидный `config.yaml` по схеме `v1` (модели, роли).

## When to Use
- Новая рабочая станция или переезд Home/Office.
- Модели исчезли из списка Continue.
- Ошибки `Failed to parse config` или `Invalid input`.
- OneDrive откатил/сконфликтовал `config.yaml`.

## Key Rules
- **SSOT**: единственный источник — `${CONTINUE_HOME}` (текущее значение: `D:\Clouds\AO\OneDrive\AI\.continue`).
  - См. `INFRASTRUCTURE_CONFIG.yaml` → `paths.continue_ssot` для актуального пути.
- **Local path** `%USERPROFILE%\.continue` должен быть **junction** на OneDrive.
- **config.ts запрещен**: при наличии может игнорировать `config.yaml`.
- `config.yaml` должен быть в схеме `v1` и содержать `models` с полями:
  - `name`, `provider`, `model`, `roles` (минимум `chat`).
- Секреты не коммитить; ключи хранятся только в локальном `config.yaml`.

## Workflow
1) Проверить OneDrive синхронизацию папки `.continue`.
2) Проверить SSOT путь:
   - `${CONTINUE_HOME}/config.yaml` существует и валиден.
   - Текущий путь: см. `INFRASTRUCTURE_CONFIG.yaml` → `paths.continue_ssot`.
3) Проверить переменные и настройки:
   - `CONTINUE_HOME` = `paths.continue_ssot` из `INFRASTRUCTURE_CONFIG.yaml`.
   - `continue.continuePath` в `settings.json` указывает на OneDrive.
4) Проверить junction:
   - CMD: `dir /AL "%USERPROFILE%"`
   - Должен быть: `JUNCTION  .continue [${CONTINUE_HOME}]`
5) Удалить `config.ts` (если появился в OneDrive).
6) Перезапустить Cursor (полный Exit/Start).

## Home Checklist
### Команды (CMD)
1) Проверить junction:
   - `dir /AL "%USERPROFILE%"`
   - Должно быть: `JUNCTION  .continue [${CONTINUE_HOME}]`
2) Если junction отсутствует — создать:
   - `if exist "%USERPROFILE%\.continue" ren "%USERPROFILE%\.continue" ".continue.backup" && mklink /J "%USERPROFILE%\.continue" "${CONTINUE_HOME}"`
   - Текущий путь: см. `INFRASTRUCTURE_CONFIG.yaml` → `paths.continue_ssot`.
3) Проверить переменную:
   - `set CONTINUE_HOME`
   - Должно соответствовать: `paths.continue_ssot` из `INFRASTRUCTURE_CONFIG.yaml`.

### UI-шаги (Cursor)
1) Полный выход из Cursor и повторный запуск.
2) В панели Continue нажать `Reload`.
3) Проверить список моделей: `mistral-small`, `ollama-llama3-2`.

## Recovery
- Если OneDrive откатил `config.yaml`:
  1) Восстановить файл из истории OneDrive.
  2) Проверить валидность `models` по схеме `v1`.
  3) Перезапустить Cursor.

## References
- `skills-mbb/skills/process/process-infrastructure-maintenance.md`
- `skills/skills/security/skill-secrets-hygiene.md`
- `%APPDATA%\Cursor\User\settings.json`
- `${CONTINUE_HOME}/config.yaml` (см. `INFRASTRUCTURE_CONFIG.yaml` → `paths.continue_ssot`)
- https://docs.continue.dev/reference

## Metadata
- tags: #process #continue #config #ssot #windows #onedrive
- dependencies: process-infrastructure-maintenance, protocol-agent-core, security/skill-secrets-hygiene
- updated_at: 2026-01-30
- source_refs: https://docs.continue.dev/reference
