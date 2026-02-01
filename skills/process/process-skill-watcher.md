---
id: process-skill-watcher
title: Process: Skill Watcher (Git)
scope: skills-mbb
tags: [#process, #automation, #git, #discovery]
priority: medium
created_at: 2026-01-31
updated_at: 2026-02-01
---

# Process: Skill Watcher (Git)

> **Context**: Analyzing Git history to discover hidden knowledge.
> **SSOT**: `scripts/skill-watcher.js`

## 1. Architecture

```
git log ──► isSkillWorthy() ──► generateExplanationLLM() ──► SKILL_CANDIDATES.json
              │                        │
              ▼                        ▼
        [Heuristics]            [Structured Prompt]
        See: process-commit-    See: process-commit-
        analysis-heuristics     skill-extraction
```

## 2. Pipeline Stages

| Stage | Input | Output | Skill Reference |
|-------|-------|--------|-----------------|
| 1. Scan | `git log` | Commit list | — |
| 2. Filter | Commit | worthy/reject | `process-commit-analysis-heuristics` |
| 3. Extract | Diff | Structured response | `process-commit-skill-extraction` |
| 4. Validate | LLM output | Valid skill or reject | `process-skill-quality-validation` |
| 5. Store | Valid skill | JSON entry | — |

## 3. Hard Constraints

1. **Size Limits**: MAX 1500 lines, MAX 15 files per commit
2. **No Duplicates**: `SCAN_REGISTRY.json` prevents re-scanning
3. **Structured Output**: LLM must follow exact response format
4. **Quality Gate**: Abstract/philosophical responses rejected

## 4. Related Skills

- `process-commit-analysis-heuristics` — Filtering rules
- `process-commit-skill-extraction` — LLM prompt protocol
- `process-skill-quality-validation` — Output validation
- `process-skills-curation-intelligence` — Meta-decisions

## 5. File Map

- `@scripts/skill-watcher.js`: Main logic
- `@mcp/continue-wrapper/server.js`: LLM API
- `@events/SKILL_CANDIDATES.json`: Output queue
- `@events/SCAN_REGISTRY.json`: Processed commits
