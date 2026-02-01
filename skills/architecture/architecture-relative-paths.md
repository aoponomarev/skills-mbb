---
id: architecture-relative-paths
title: Architecture: Relative Paths (EEIIPP)
scope: skills-mbb
tags: [#architecture, #eeiipp, #ssot, #infrastructure]
priority: high
created_at: 2026-01-30
updated_at: 2026-02-01
---

# Architecture: Relative Paths (EEIIPP)

> **Context**: Portability principle ensuring the project runs on any machine (Home/Office) without config changes.
> **Rule**: "EEIIPP" (Single Source of Truth / Portable Paths).

## 1. Core Principle
All paths MUST be **relative** to the project root or the current file.
**FORBIDDEN**: Absolute paths (`C:\`, `D:\`, `/Users/...`) in configs or code.

## 2. Implementation Rules

### Infrastructure (`INFRASTRUCTURE_CONFIG.yaml`, `.env`)
- Use `.` for current dir, `..` for parent.
- Bad: `D:\Projects\MBB`
- Good: `./MBB` (relative to root).

### Scripts (`*.js`, `*.py`)
- Resolve root dynamically:
  ```javascript
  const root = path.resolve(__dirname, '..');
  ```
- Do not hardcode drive letters.

### Documentation (`*.md`)
- Links must be relative: `[Link](../folder/file.md)`.
- Runnable examples should use env vars (`$PROJECT_ROOT`).

### Docker
- Volume mappings must be relative:
  ```yaml
  volumes:
    - ../skills:/workspace/skills
  ```

## 3. Why?
1.  **Portability**: Works on any drive/OS.
2.  **Sync**: Seamless OneDrive sync between devices.
3.  **Safety**: Prevents accidental writes to wrong absolute paths.

## 4. Verification
Command `ееиипп` triggers a scan for absolute paths in the codebase.
