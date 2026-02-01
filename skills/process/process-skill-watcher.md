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
1.  **Scan**: `git log` for recent commits.
2.  **Filter**: `isSkillWorthy()` heuristic (file types, keywords).
3.  **Analyze**: `git show` -> LLM Prompt -> "Why is this a pattern?".
4.  **Report**: Append to `SKILL_CANDIDATES.json`.

## 2. LLM Integration
- **Input**: Full Commit Diff (max 3500 chars).
- **Prompt**: "Identify architectural pattern and explain value".
- **Output**: Short Russian/English explanation.

## 3. Hard Constraints
1.  **Token Limit**: Diffs truncated to prevent context overflow.
2.  **No Duplicates**: Checks `SCAN_REGISTRY.json` to avoid re-scanning.
3.  **Fallback**: If LLM fails, use template description.

## 4. File Map
- `@scripts/skill-watcher.js`: The Watcher.
- `@events/SKILL_CANDIDATES.json`: Output Queue.
