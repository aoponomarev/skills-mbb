---
id: process-skills-bridge
title: Process: Skills UI Bridge
scope: skills-mbb
tags: [#process, #ui, #bridge, #curation]
priority: high
created_at: 2026-01-28
updated_at: 2026-02-01
---

# Process: Skills UI Bridge

> **Context**: The visual interface for Knowledge Management.
> **SSOT**: `mcp/NEW_SKILLS.html`, `mcp/skills-ui.js`

## 1. Queues
1.  **Candidates**: From Git History. Action: Approve/Reject.
2.  **User Ideas**: From UI Input. Action: Plan & Execute.
3.  **Drafts**: Generated Skills. Action: Publish (Move to `skills/`).

## 2. Agent Workflow
- **Monitor**: Check `USER_IDEAS.json`. If not empty -> Priority Task.
- **Suggest**: If `SKILL_CANDIDATES` has entries -> Suggest review.

## 3. Status Lifecycle
`candidate` -> `draft` -> `promoted` (Published).

## 4. File Map
- `@mcp/NEW_SKILLS.html`: UI Frontend.
- `@mcp/skills-mcp/server.js`: Backend API.
