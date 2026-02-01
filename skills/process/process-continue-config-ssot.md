---
id: process-continue-config-ssot
title: Process: Continue Config SSOT
scope: skills-mbb
tags: [#process, #continue, #config, #ssot]
priority: high
created_at: 2026-01-30
updated_at: 2026-02-01
---

# Process: Continue Config SSOT

> **Context**: Managing the `config.yaml` for Continue across multiple devices.

## 1. The Junction
**Rule**: `%USERPROFILE%\.continue` MUST be a Directory Junction to `[OneDrive]\AI\.continue`.
This ensures that settings, prompts, and model configs are synced automatically.

## 2. File Roles
- **`config.yaml`**: The ONLY source of truth for models and tools.
- **`config.ts`**: FORBIDDEN. If found, delete it immediately.
- **`config.json`**: Legacy. Do not use.

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
