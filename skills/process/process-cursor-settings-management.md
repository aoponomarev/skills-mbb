---
id: process-cursor-settings-management
title: "Управление настройками Cursor IDE"
description_ru: "Протокол централизованного управления настройками Cursor: workspace и глобальными. Резервное копирование, восстановление, синхронизация между машинами"
scope: "Обеспечение воспроизводимости конфигурации Cursor IDE при переустановке или переезде на другую машину"
tags: [#process, #infrastructure, #cursor, #settings, #devops]
priority: medium
created_at: 2026-01-29
updated_at: 2026-01-29
source: "mcp:settings-optimization"
dependencies: ["process-infrastructure-maintenance"]
---

# Управление настройками Cursor IDE

> **Skill ID:** `process-cursor-settings-management`

## Scope

Данный навык описывает:
1. Структуру и расположение настроек Cursor
2. Разделение workspace и глобальных настроек
3. Процедуры резервного копирования
4. Восстановление настроек после переустановки

## Расположение файлов настроек

### Workspace-уровень (проект MBB)

| Файл | Назначение |
|------|------------|
| `.vscode/settings.json` | Настройки проекта |
| `.vscode/settings.backup.json` | Резервная копия |
| `.vscode/keybindings.json` | Сочетания клавиш проекта |
| `.vscode/cspell-dict.txt` | Словарь проверки орфографии |
| `.cursorrules` | Правила поведения AI-агента |

### Глобальный уровень (User)

| Путь (Windows) | Назначение |
|----------------|------------|
| `%APPDATA%\Cursor\User\settings.json` | Глобальные настройки |
| `%APPDATA%\Cursor\User\keybindings.json` | Глобальные сочетания клавиш |
| `%APPDATA%\Cursor\User\snippets\` | Пользовательские сниппеты |

### Резервная копия в проекте

Глобальные настройки продублированы в `INFRASTRUCTURE_CONFIG.yaml` в секции `cursor_global_settings`.

## Категории настроек Cursor

### 1. AI Agent (cursor.*)

```json
{
  "cursor.chat.autoScroll": true,
  "cursor.chat.largeContext": true,
  "cursor.chat.iterateOnLints": true,
  "cursor.chat.collapseInputBoxPills": true,
  "cursor.terminal.usePreviewBox": true,
  "cursor.tab.partialAccepts": true,
  "cursor.tab.suggestionsInComments": true,
  "cursor.tab.autoImport": true,
  "cursor.composer.enabled": true,
  "cursor.general.enableShadowWorkspace": true
}
```

### 2. Continue Extension

```json
{
  "continue.enableConsole": true,
  "continue.enableQuickActions": true
}
```

### 3. Editor Core

```json
{
  "editor.fontFamily": "'JetBrains Mono', 'Source Code Pro', Consolas, monospace",
  "editor.fontSize": 14,
  "editor.fontLigatures": true,
  "editor.minimap.enabled": false,
  "editor.stickyScroll.enabled": true,
  "editor.inlineSuggest.enabled": true,
  "editor.bracketPairColorization.enabled": true
}
```

### 4. Workspace Behavior

```json
{
  "workbench.colorTheme": "Prism (No Bold)",
  "workbench.startupEditor": "none",
  "workbench.editor.enablePreview": false,
  "workbench.localHistory.enabled": true,
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false
}
```

### 5. Privacy & Telemetry

```json
{
  "telemetry.telemetryLevel": "off",
  "redhat.telemetry.enabled": false,
  "security.workspace.trust.enabled": false
}
```

## Процедуры

### При изменении настроек

1. Внести изменения в `.vscode/settings.json`
2. Если настройка глобальная — обновить `%APPDATA%\Cursor\User\settings.json`
3. Обновить копию в `INFRASTRUCTURE_CONFIG.yaml` → секция `cursor_global_settings`
4. Закоммитить изменения

### При переустановке Windows/Cursor

1. Установить Cursor
2. Скопировать содержимое `cursor_global_settings` из `INFRASTRUCTURE_CONFIG.yaml`
3. Вставить в `%APPDATA%\Cursor\User\settings.json`
4. Открыть проект MBB — workspace настройки применятся автоматически

### При синхронизации между машинами

**Вариант A: Встроенная синхронизация Cursor**
- Profile → Turn on Settings Sync
- Выбрать что синхронизировать (Settings, Keybindings, Extensions)

**Вариант B: Ручная через Git**
- `.vscode/settings.json` уже в репозитории
- Глобальные настройки копировать из `INFRASTRUCTURE_CONFIG.yaml`

## Ограничения

**Нельзя настроить через JSON:**
- Настройки аккаунта (Manage Account)
- Data Sharing (Privacy) — только через UI
- Некоторые YOLO mode параметры

**Cursor не документирует публично:**
- Полный список ключей `cursor.*`
- Некоторые экспериментальные настройки

## Связанные файлы

- `INFRASTRUCTURE_CONFIG.yaml` — содержит backup глобальных настроек
- `.cursorrules` — правила поведения AI-агента (Communication Protocol)
- `docs/INFRASTRUCTURE_RECOVERY.md` — общий план восстановления

## Связанные навыки

- `process-infrastructure-maintenance` — общий протокол инфраструктуры
- `protocol-agent-core` — протокол поведения агента
