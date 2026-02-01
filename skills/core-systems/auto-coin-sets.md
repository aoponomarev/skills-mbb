---
id: auto-coin-sets
title: Core: Automated Coin Sets
scope: skills-mbb
tags: [#core, #automation, #coins, #logic]
priority: medium
created_at: 2026-01-25
updated_at: 2026-02-01
---

# Core: Automated Coin Sets

> **Context**: Dynamic generation of coin lists based on market criteria.
> **SSOT**: `core/utils/auto-coin-sets.js`

## 1. Purpose
Eliminates manual portfolio building by automatically grouping coins (e.g., "Top 10 L1s", "Stablecoins", "High Volume").

## 2. Logic Flow
1.  **Rules**: Defined in `auto-coin-sets.js` (e.g., `market_cap > 1B`).
2.  **Source**: Fetches from `CacheManager` (`coins-list`).
3.  **Generation**: `draft-coin-set.js` creates a temporary portfolio object.
4.  **UI**: User previews and "promotes" to a real Portfolio.

## 3. Hard Constraints
1.  **Centralized Rules**: No hardcoded filtering in Vue components.
2.  **Consistency**: Sets must be reproducible from the same market snapshot.

## 4. File Map
- `@core/utils/auto-coin-sets.js`: Filtering rules.
- `@core/utils/draft-coin-set.js`: Object factory.
