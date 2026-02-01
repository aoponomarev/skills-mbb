---
id: process-commit-skill-extraction
title: Process: Commit Skill Extraction Protocol
scope: skills-mbb
tags: [#process, #extraction, #llm, #commits]
priority: high
created_at: 2026-02-01
updated_at: 2026-02-01
---

# Process: Commit Skill Extraction Protocol

> **Context**: Structured protocol for extracting actionable skills from git commits.
> **SSOT**: `scripts/skill-watcher.js` (prompt section)

## 1. Pre-Extraction Filters

Before LLM analysis, commits are filtered:

| Filter | Threshold | Reason |
|--------|-----------|--------|
| `MAX_LINES_CHANGED` | 1500 | Multi-feature commits can't yield single skill |
| `MAX_FILES_CHANGED` | 15 | Too scattered = infrastructure dump |
| `NOISE_PATTERNS` | package-lock, .sqlite | Generated files don't contain patterns |

**If filtered out**: Return `SKILL_EXTRACTABLE: NO` with reason.

## 2. Response Format (Strict)

### Format A: Skill Extracted
```
SKILL_EXTRACTABLE: YES
SKILL_ID: kebab-case-identifier
CATEGORY: [architecture|integration|automation|security|debugging|process]
TITLE_RU: Краткое название (3-5 слов)
DESCRIPTION: 2-3 sentences. WHAT the pattern does. HOW to apply it.
```

### Format B: No Skill
```
SKILL_EXTRACTABLE: NO
REASON: [multi_feature_commit|trivial_change|config_only|no_reusable_pattern|unclear_intent]
EXPLANATION: 1 sentence why.
```

## 3. Category Definitions

| Category | When to Use |
|----------|-------------|
| `architecture` | Structural patterns, component organization, data flow |
| `integration` | External APIs, services, third-party libs |
| `automation` | Scripts, CI/CD, scheduled tasks |
| `security` | Auth, secrets, permissions |
| `debugging` | Error handling, logging, troubleshooting |
| `process` | Workflows, protocols, team conventions |

## 4. Quality Criteria

A valid skill MUST be:
1. **Actionable** — Agent can follow concrete steps
2. **Reusable** — Applies beyond this specific commit
3. **Focused** — One pattern, not a philosophy essay
4. **Verifiable** — Success/failure can be determined

## 5. Anti-Patterns (Reject These)

❌ "This demonstrates scalable architecture..." — Too abstract  
❌ "The pattern ensures resilience..." — Philosophy, not instruction  
❌ "UPDATE [nonexistent-skill-id]:" — Hallucinated reference  
❌ Multi-paragraph essays without structure — Not actionable

## 6. File Map

- `@scripts/skill-watcher.js`: Extraction logic
- `@mcp/continue-wrapper/server.js`: LLM API calls
- `@events/SKILL_CANDIDATES.json`: Extraction results
