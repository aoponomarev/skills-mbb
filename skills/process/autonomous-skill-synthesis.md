---
id: autonomous-skill-synthesis
title: Process: Autonomous Skill Synthesis
scope: skills-mbb
tags: [#process, #skills, #automation, #ai]
priority: high
created_at: 2026-01-28
updated_at: 2026-02-01
---

# Process: Autonomous Skill Synthesis

> **Context**: How the system automatically transforms raw logs into structured knowledge.

## 1. The Pipeline
1.  **Watcher**: `skill-watcher.js` scans Git diffs for "worthy" patterns.
2.  **Drafter**: `skill-processor.js` sends the diff to an LLM.
3.  **Synthesis**: LLM writes a new `.md` file in `drafts/` using the standard template.

## 2. Selection Heuristics
A change is "Skill-worthy" if it:
- Fixes a recurring architectural bug.
- Introduces a new integration pattern.
- Defines a new naming convention.
- Solves a complex environment issue (Docker/WSL).

## 3. Hard Constraints
1.  **Human Gate**: No skill is moved from `drafts/` to `skills/` without manual approval.
2.  **Context Limit**: Diffs are capped at 3500 characters to ensure LLM focus.

## 4. File Map
- `@scripts/skill-watcher.js`: The Scanner.
- `@scripts/skill-processor.js`: The Drafter.
