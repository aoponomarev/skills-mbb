---
id: process-batch-skills-review
title: Process: Batch Skills Review
scope: skills-mbb
tags: [#process, #skills, #maintenance, #quality]
priority: medium
created_at: 2026-01-28
updated_at: 2026-02-01
---

# Process: Batch Skills Review

> **Context**: Periodic audit of the knowledge base to prevent rot.

## 1. Audit Steps
1.  **Dead Links**: Scan all `.md` files for broken relative paths.
2.  **Redundancy**: Identify skills that cover similar topics and `MERGE` them.
3.  **Staleness**: Check `updated_at` dates. Skills older than 90 days need re-validation.
4.  **Index Sync**: Ensure `index-mbb.md` contains all active skills.

## 2. Workflow
- **Trigger**: Monthly or after major architectural shifts.
- **Action**: Create `action=revalidate` entries in `BACKLOG.md`.

## 3. Hard Constraints
1.  **No Orphans**: Every skill MUST be linked from at least one index.
2.  **Consistency**: All skills in a batch must follow the latest `process-skill-template.md`.

## 4. File Map
- `@skills-mbb/skills/index/index-mbb.md`: The Map.
