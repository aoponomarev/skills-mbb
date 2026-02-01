---
id: process-settings-sync
title: Process: Settings Sync (Master)
scope: skills-mbb
tags: [#process, #sync, #cursor, #continue, #ssot]
priority: critical
created_at: 2026-02-02
updated_at: 2026-02-02
---

# Process: Settings Sync (Master)

> **Context**: Unified synchronization of ALL settings between Home/Office PCs via OneDrive.
> **SSOT**: This skill is the single source of truth for settings sync. Other skills (`process-cursor-settings-management`, `process-continue-config-ssot`) are subordinate.

## 1. What Gets Synced

| Category | Files | SSOT Location | Local Location |
|----------|-------|---------------|----------------|
| **Cursor IDE** | `settings.json`, `keybindings.json` | `D:\Clouds\AO\OneDrive\AI\Cursor\User\` | `%APPDATA%\Cursor\User\` |
| **Continue** | `config.yaml`, `.continuerc.json`, `.continueignore` | `D:\Clouds\AO\OneDrive\AI\.continue\` | `%USERPROFILE%\.continue\` |
| **Project** | `.cursorrules` | `D:\Clouds\AO\OneDrive\AI\Cursor\User\` | `MBB\.cursorrules` |
| **Secrets** | `.env` | `D:\Clouds\AO\OneDrive\AI\MBB\` | `MBB\.env` |

## 2. Sync Script

**Command**: `powershell .\scripts\sync-cursor-settings.ps1 [action]`

| Action | Description |
|--------|-------------|
| `restore` | OneDrive → Local (use at session START) |
| `backup` | Local → OneDrive (use at session END) |
| `diff` | Compare hashes, show differences |
| `status` | Show timestamps of all files |

## 3. Agent Protocol

### 3.1. Session Start (Auto-Restore)
Agent MUST execute at the beginning of every session:
```
1. Run: powershell .\scripts\sync-cursor-settings.ps1 restore
2. If .env in OneDrive is newer → copy to MBB root
3. Restart Cursor if config.yaml changed
```

### 3.2. Session End (Auto-Backup)
Agent MUST execute when user says "Закрываем рабочий день", "Копируй настройки в облако", or at explicit session termination:
```
1. Run: powershell .\scripts\sync-cursor-settings.ps1 backup
2. If local .env changed → copy to OneDrive
3. Confirm: "✅ Cloud SSOT updated"
```

## 4. What is NOT Synced

- `%USERPROFILE%\.continue\index\` — SQLite databases (heavy, causes OneDrive locks)
- `%USERPROFILE%\.continue\cache\` — temporary data
- `%USERPROFILE%\.continue\sessions\` — session history
- `%APPDATA%\Cursor\User\state.vscdb` — SQLite state
- `%APPDATA%\Cursor\User\globalStorage\` — extensions data

## 5. Hard Constraints

1. **Copy, not Junction**: Directory junctions are FORBIDDEN for `.continue` (SQLite locks).
2. **No Direct Edits**: Always edit SSOT in OneDrive, then run `restore`.
3. **Secrets Hygiene**: `.env` is synced but NEVER committed to Git.

## 6. Trigger Words

Agent should recognize these phrases and act accordingly:
- **Restore**: "начинаем сессию", "синхронизируй из облака", "restore settings"
- **Backup**: "закрываем рабочий день", "копируй настройки в облако", "backup settings", "сохрани в облако"

## 7. Related Skills

- `process-cursor-settings-management` — Cursor-specific details
- `process-continue-config-ssot` — Continue-specific details
- `skill-secrets-hygiene` — .env handling rules
- `process-session-handoff` — Full session termination protocol

## 8. File Map

- `@INFRASTRUCTURE_CONFIG.yaml` — All paths and profiles
- `@scripts/sync-cursor-settings.ps1` — Sync script implementation
- `@scripts/SYNC_CURSOR_README.md` — User documentation
