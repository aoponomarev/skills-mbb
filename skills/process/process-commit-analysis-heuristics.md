---
id: process-commit-analysis-heuristics
title: Process: Commit Analysis Heuristics
scope: skills-mbb
tags: [#process, #heuristics, #git, #filtering]
priority: medium
created_at: 2026-02-01
updated_at: 2026-02-01
---

# Process: Commit Analysis Heuristics

> **Context**: Rules for determining if a commit is worth analyzing for skill extraction.
> **SSOT**: `scripts/skill-watcher.js` → `isSkillWorthy()`

## 1. Worthiness Decision Tree

```
Commit
  │
  ├─ Lines > 1500? ──────────────────► REJECT (too_large)
  │
  ├─ Files > 15? ────────────────────► REJECT (too_many_files)
  │
  ├─ All files are .md or docs/? ────► REJECT (docs_only)
  │
  ├─ Lines > 100? ───────────────────► ACCEPT (significant_changes)
  │
  ├─ Has keyword in subject? ────────► ACCEPT (keyword_match)
  │   (implement, integrate, refactor, fix, add, create, auth, api, cache)
  │
  ├─ Touches core dirs + >2 files? ──► ACCEPT (core_changes)
  │   (core/, cloud/, mcp/, app/)
  │
  └─ Otherwise ──────────────────────► REJECT (not_significant)
```

## 2. Noise Patterns (Always Filtered)

These files are excluded from analysis:
- `package-lock.json` — Generated dependency tree
- `node_modules/` — External code
- `*.sqlite`, `*.sqlite-*` — Binary databases
- `*.min.js`, `*.min.css` — Minified assets
- `workflows_export.json` — n8n backup dumps

## 3. Rejection Reasons

| Reason | Meaning | Action |
|--------|---------|--------|
| `too_large` | >1500 lines changed | Split commit or skip |
| `too_many_files` | >15 files touched | Likely multi-feature |
| `docs_only` | Only documentation | No code pattern |
| `not_significant` | <100 lines, no keywords | Trivial change |

## 4. Acceptance Reasons

| Reason | Meaning |
|--------|---------|
| `significant_changes` | Substantial code modification |
| `keyword_match` | Commit message indicates feature work |
| `core_changes` | Touches critical system directories |

## 5. Tuning Parameters

Located in `CONFIG` object:
```javascript
MIN_LINES_CHANGED: 100,   // Minimum for "significant"
MAX_LINES_CHANGED: 1500,  // Maximum before "too_large"
MAX_FILES_CHANGED: 15,    // Maximum before "too_many_files"
```

Adjust based on project velocity and commit hygiene.

## 6. File Map

- `@scripts/skill-watcher.js`: Heuristic implementation
- `@events/SCAN_REGISTRY.json`: Tracks scanned commits
