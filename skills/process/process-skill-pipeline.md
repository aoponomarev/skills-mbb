---
id: process-skill-pipeline
title: Process: Automated Skill Pipeline
scope: skills-mbb
tags: [#process, #skills, #automation, #llm]
priority: high
created_at: 2026-01-27
updated_at: 2026-02-01
---

# Process: Automated Skill Pipeline

> **Context**: Converting BACKLOG items into Skill Drafts via AI.
> **SSOT**: `scripts/skill-processor.js`

## 1. Architecture
`BACKLOG.md` -> `skill-processor.js` -> `Continue CLI` -> `drafts/*.md`

## 2. Workflow
1.  **Trigger**: Entry in `BACKLOG.md` with `[action=create] [status=pending]`.
2.  **Process**: Script reads entry, constructs prompt using `list_skills` context.
3.  **Generate**: Calls Mistral (via Wrapper) to write Markdown.
4.  **Save**: Writes to `drafts/` and updates status to `[status=drafted]`.

## 3. Why Node.js?
Replaced n8n workflow because:
- Direct file access stability.
- Simplified debugging (`node skill-processor.js`).
- No complex `Execute Command` permission issues.

## 4. Hard Constraints
1.  **Format**: BACKLOG entries must strictly follow the regex pattern (title, scope, tags).
2.  **Output**: Generated files must include valid YAML front-matter.
3.  **Review**: Humans MUST review drafts before moving to `skills/`.

## 5. File Map
- `@scripts/skill-processor.js`: The Engine.
- `@skills-mbb/BACKLOG.md`: Input Queue.
