---
id: architecture-relative-paths
title: "Architecture: Relative Paths (EEIIPP)"
description_ru: "Принцип использования относительных путей для переносимости инфраструктуры и соблюдения ееиипп."
scope: "Enforce relative paths across documentation, scripts, and infrastructure configs to ensure system portability."
tags: [#architecture, #eeiipp, #ssot, #infrastructure]
priority: high
created_at: 2026-01-30
updated_at: 2026-01-30
source: "agent:eeiipp-path-standard"
---

# Architecture: Relative Paths (EEIIPP)

## Core Principle (ееиипп)
Все пути в проекте должны быть **относительными** относительно корня проекта или текущего файла. Использование абсолютных путей (начинающихся с `C:\`, `D:\`, `/Users/...`) строго запрещено, так как это нарушает принцип переносимости и "Единого Источника Правды" (ееиипп).

## Implementation Rules

### 1. Infrastructure Configs (`INFRASTRUCTURE_CONFIG.yaml`, `.env`)
- Используйте `.` для текущей директории и `..` для родительской.
- Вместо `D:\...\Statistics\MBB` используйте `./` (если конфиг в корне MBB).
- Вместо `D:\...\Statistics\libs` используйте `../libs`.

### 2. Automation Scripts (`scripts/*.js`, `*.py`, `*.sh`)
- Используйте динамическое определение корня проекта через `__dirname` (Node.js) или `os.path` (Python).
- Все пути внутри скриптов должны строиться от вычисленного корня.

### 3. Documentation (`*.md`)
- Ссылки на файлы проекта должны быть относительными: `[README](../README.md)`.
- Избегайте упоминания полных путей в примерах команд, используйте переменные окружения или относительные указатели.

### 4. Docker & Containers
- Маппинг томов в `docker-compose.yml` всегда должен быть относительным:
  ```yaml
  volumes:
    - ../skills:/workspace/skills
  ```

## Why this matters
Соблюдение этого правила позволяет:
1. Запускать проект на любом диске и в любой папке без правок конфигов.
2. Бесшовно переходить между домашним и рабочим окружением.
3. Избегать ошибок при переименовании родительских папок (как в случае с `Portfolio - CV` -> `Portfolio-CV`).

## Verification
При использовании команды `ееиипп`, агент обязан просканировать изменения на наличие абсолютных путей и заменить их на относительные.
