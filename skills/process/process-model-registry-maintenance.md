---
id: process-model-registry-maintenance
title: Process: Model Registry Sync
scope: skills-mbb
tags: [#process, #maintenance, #llm, #ssot]
priority: high
created_at: 2026-01-30
updated_at: 2026-02-01
---

# Process: Model Registry Sync

> **Context**: Synchronization between technical config and human-readable registry.
> **SSOT**: `.continue/config.yaml` (Tech), `logs/llm-models-registry.md` (Human).

## 1. Sync Rule
**Config <-> Registry**: Every change in `config.yaml` (new model, key change, comment out) MUST be reflected in `logs/llm-models-registry.md`.

## 2. Maintenance Tasks
1.  **Field Data**: Record speed (tok/s) and error rates.
2.  **Tier Updates**: Downgrade models that start failing (e.g. rate limits).
3.  **New Models**: Add promising models to "Candidates" section.

## 3. Tier Structure
- **Tier 1 (Premium)**: Proven paid models (Mistral).
- **Tier 2 (Advanced Free)**: High-speed daily drivers (Groq).
- **Tier 3 (Specialized)**: Coding/Reasoning specific (DeepSeek).
- **Tier 4 (Standard)**: Backup/Slow models.

## 4. Hard Constraints
1.  **No Ghost Models**: If it's in config, it must be in registry.
2.  **Fallback Awareness**: Update the "Fallback Algorithm" diagram in registry if logic changes in `server.js`.

## 5. File Map
- `@.continue/config.yaml`: The Source.
- `@logs/llm-models-registry.md`: The Documentation.
