---
id: process-skill-watcher
title: Process: Skill Watcher (Git)
scope: skills-mbb
tags: [#process, #automation, #git, #discovery]
priority: medium
created_at: 2026-01-31
updated_at: 2026-02-02
---

# Process: Skill Watcher (Git)

> **Context**: Analyzing Git history to discover hidden knowledge.
> **SSOT**: `scripts/skill-watcher.js`

## 1. Architecture

```
git log ──► isSkillWorthy() ──► generateExplanationLLM() ──► SKILL_CANDIDATES.json
              │                        │
              │                        └──► mcpRelevanceCheck() ──► MCP_SERVER_CANDIDATES.json
              ▼
        [Heuristics]
        See: process-commit-
        analysis-heuristics
```

## 2. Pipeline Stages

| Stage | Input | Output | Skill Reference |
|-------|-------|--------|-----------------|
| 1. Scan | `git log` | Commit list | — |
| 2. Filter | Commit | worthy/reject | `process-commit-analysis-heuristics` |
| 3. Extract | Diff | Structured response | `process-commit-skill-extraction` |
| 4. Validate | LLM output | Valid skill or reject | `process-skill-quality-validation` |
| 5. Store | Valid skill | JSON entry | — |
| 6. MCP Reflect | Commit + LLM summary | MCP candidates | `process-unified-mcp-orchestration` |

## 3. MCP Reflection (Catalog)

**Goal**: from each commit summary decide whether a Docker MCP Catalog server could reduce effort, increase automation, or improve observability.

**Sources**:
- Official catalog: https://hub.docker.com/mcp/explore
- Docs: https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog/

**Triggers** (non-exhaustive):
1. **Repo automation**: GitHub, Git, CI/CD, release tasks
2. **Docs/KB**: Notion, Confluence, Google Docs, wiki tasks
3. **Observability**: Grafana, Datadog, New Relic, logs/metrics
4. **Search/Research**: web search, docs search, data discovery
5. **Browser automation**: Playwright, scraping, QA
6. **Data/DB**: MongoDB, Postgres/Neon, Elasticsearch

**Output schema** (`events/MCP_SERVER_CANDIDATES.json`):
```
[
  {
    "commit": "<sha>",
    "title": "<subject>",
    "reason": "<why MCP helps>",
    "category": "<catalog category>",
    "suggested_servers": ["<server-name>"],
    "links": ["https://hub.docker.com/mcp/explore"]
  }
]
```

## 4. Hard Constraints

1. **Size Limits**: MAX 1500 lines, MAX 15 files per commit
2. **No Duplicates**: `SCAN_REGISTRY.json` prevents re-scanning
3. **Structured Output**: LLM must follow exact response format
4. **Quality Gate**: Abstract/philosophical responses rejected

## 5. Related Skills

- `process-commit-analysis-heuristics` — Filtering rules
- `process-commit-skill-extraction` — LLM prompt protocol
- `process-skill-quality-validation` — Output validation
- `process-skills-curation-intelligence` — Meta-decisions
- `process-unified-mcp-orchestration` — MCP selection & routing rules

## 6. File Map

- `@scripts/skill-watcher.js`: Main logic
- `@mcp/continue-wrapper/server.js`: LLM API
- `@events/SKILL_CANDIDATES.json`: Output queue
- `@events/SCAN_REGISTRY.json`: Processed commits
- `@events/MCP_SERVER_CANDIDATES.json`: MCP opportunities backlog
