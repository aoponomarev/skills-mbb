---
id: integrations-data-provider-resilience
title: Data Provider Resilience & Health Monitoring
description_ru: Протоколы обеспечения отказоустойчивости и мониторинга здоровья для внешних поставщиков данных (API).
scope: integrations, reliability
tags: [cryptocurrency, data provider, resilience, stability, monitoring, API]
priority: medium
created_at: 2026-02-01
updated_at: 2026-02-01
---

# Data Provider Resilience & Health Monitoring

## Overview
External APIs (e.g., CoinGecko, DexScreener) are critical for system operation. This skill defines how to monitor their health, track response latency, and implement fallback logic when providers fail.

## Key Metrics Monitored
- **API Uptime**: Percentage of successful responses.
- **Latency**: Time-to-first-byte and total response time (tracked in `avgLatency`).
- **Data Consistency**: Cross-referencing values from different providers.
- **Rate Limit Compliance**: Handling 429 errors and exponential backoff.

## Resilience Steps

### 1. Health Tracking
- Store successes/failures per provider in a registry (e.g., `llm-model-stats.json` pattern).
- Log latency for every call to detect degradation before a total outage.

### 2. Fallback Logic
- Implement a fallback chain:
  ```javascript
  const PROVIDER_CHAIN = ['coingecko', 'dexscreener', 'binance'];
  ```
- Use the healthiest provider first based on recent metrics.

### 3. Graceful Degradation
- If all providers are down, serve cached data from `LLM_MODELS_CACHE.json` or local SQLite.
- Notify the UI with a "Stale Data" warning.

## Validation Criteria
- API uptime should be maintained at >99.5%.
- Average response time should stay under defined thresholds (e.g., 1s).
- Fallback must trigger within 1 retry of a failed call.
