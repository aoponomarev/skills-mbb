---
id: process-cursor-settings-management
title: Process: Cursor Settings Sync
scope: skills-mbb
tags: [#process, #cursor, #config, #sync]
priority: medium
created_at: 2026-01-30
updated_at: 2026-02-01
---

# Process: Cursor Settings Sync

> **Context**: Managing IDE settings and extensions across Home/Office PCs.

## 1. Scope
- `settings.json`: Keybindings, UI preferences.
- `extensions`: List of mandatory plugins.

## 2. Sync Strategy
- **Manual Backup**: Periodically copy `settings.json` to `docs/cursor/`.
- **SSOT**: `INFRASTRUCTURE_CONFIG.yaml` defines the path to the local settings folder for each profile.

## 3. Key Settings
- **`editor.formatOnSave`**: Always `true`.
- **`cursor.cpp.enable`**: Always `true` for MBB.

## 4. Hard Constraints
1.  **No Secrets**: Ensure `settings.json` does not contain sensitive API keys (use `.env` instead).

## 5. File Map
- `@INFRASTRUCTURE_CONFIG.yaml`: Profile-specific paths.
