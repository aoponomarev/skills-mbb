---
title: metrics-portfolio-structure
tags:
  - "#mbb-spec"
  - "#metrics"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Metrics Portfolio Structure functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# metrics-portfolio-structure

> Источник: `docs/doc-architect.md` (раздел "Портфельная система")

## Структура портфеля

```javascript
{
    id: 'YYMMDD-hhmm',
    name: 'Portfolio Name',
    coins: [
        {
            coinId, ticker, name, rank,
            currentPrice, pvs, metrics,
            portfolioPercent,
            delegatedBy: { modelId, modelName, agrAtDelegation, timestamp }
        }
    ],
    marketMetrics: { fgi, btcDominance },
    settings: { horizonDays, mdnHours, agrMethod, mode },
    modelMix: { modelId: { coinsCount, totalPercent, modelName } }
}
```

## Хранение

`localStorage` с ключом `mbb-portfolios`. Интеграция с Cloudflare D1 — перспектива.
