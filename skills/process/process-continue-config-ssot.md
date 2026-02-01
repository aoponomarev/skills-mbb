---
id: process-continue-config-ssot
title: Process: Continue Config SSOT
scope: skills-mbb
tags: [#process, #continue, #config, #ssot]
priority: high
created_at: 2026-01-30
updated_at: 2026-02-02
---

# Process: Continue Config SSOT

> **Context**: Managing the `config.yaml` for Continue across multiple devices.
> **Master Skill**: See `process/process-settings-sync` for unified sync protocol.

## 1. Sync Method
**Rule**: Use `scripts/sync-cursor-settings.ps1` for bidirectional sync (copy-method).
Directory Junctions for `.continue` are **FORBIDDEN** to prevent SQLite database locks and performance issues with OneDrive.

## 2. File Sync Scope
Sync ONLY the following configuration files:
- `config.yaml` (Main models/tools config)
- `.continuerc.json` (Custom commands)
- `.continueignore` (Context filtering)

**DO NOT** sync the `index/` or `cache/` directories (heavy SQLite databases).

## 3. Maintenance
When updating models:
1.  Edit `[OneDrive]\AI\.continue\config.yaml`.
2.  Restart Cursor/Continue.
3.  Update `logs/llm-models-registry.md`.

## 4. Hard Constraints
1.  **No Direct Edits**: Never edit the file in `%USERPROFILE%` directly; always use the OneDrive source.
2.  **Schema v1**: Stick to the stable v1 YAML schema.

## 5. File Map
- `@INFRASTRUCTURE_CONFIG.yaml`: Defines the `continue_ssot` path.
