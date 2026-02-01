---
id: integrations-data-providers
title: Integrations: Data Providers
scope: skills-mbb
tags: [#integrations, #data-providers, #coingecko, #yahoo]
priority: high
created_at: 2026-01-25
updated_at: 2026-02-01
---

# Integrations: Data Providers

> **Context**: Unified interface for market data ingestion.
> **SSOT**: `core/config/data-providers-config.js`

## 1. Architecture
- **Abstract Base**: `BaseDataProvider` defines the contract (`getTopCoins`, `searchCoins`).
- **Implementations**: `CoinGeckoProvider`, `YahooProvider` (future).
- **Manager**: `DataProviderManager` handles provider switching and API key injection.

## 2. Key Rules
1.  **Normalization**: All provider responses MUST be normalized to the MBB internal schema via `normalizeCoinData()`.
2.  **No Direct Calls**: UI components must use `dataProviderManager.getTopCoins()`, never fetch from the provider directly.
3.  **Proxying**: `buildUrl()` method automatically routes requests through Cloudflare Worker when on `file://`.

## 3. Workflow
1.  **Add**: Create `core/api/data-providers/{name}-provider.js`.
2.  **Register**: Add to `data-provider-manager.js`.
3.  **Config**: Define limits and URLs in `data-providers-config.js`.

## 4. File Map
- `@core/api/data-provider-manager.js`: The Switcher.
- `@core/api/data-providers/base-provider.js`: The Interface.
- `@core/api/data-providers/coingecko-provider.js`: The Implementation.
