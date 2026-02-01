---
id: integrations-rate-limiting
title: Integrations: Rate Limiting
scope: skills-mbb
tags: [#integrations, #rate-limit, #api, #throttling]
priority: high
created_at: 2026-01-25
updated_at: 2026-02-01
---

# Integrations: Rate Limiting

> **Context**: Centralized request throttling to prevent API bans (429).
> **SSOT**: `core/api/rate-limiter.js`

## 1. Architecture
- **Token Bucket**: Adaptive limiter with `requestsPerMinute` and `requestsPerSecond`.
- **Global Instance**: `RateLimiter.getOrCreate(key)` ensures all components share the same budget for a given API.

## 2. API Usage
```javascript
const limiter = window.RateLimiter.getOrCreate('coingecko', 15, 0.5);
await limiter.waitForToken();
try {
    const res = await fetch(...);
    if (res.ok) limiter.decreaseTimeout(); // Good behavior reward
} catch (e) {
    if (e.status === 429) limiter.increaseTimeout(); // Penalty
}
```

## 3. Key Rules
1.  **Central Config**: Limits are defined in `data-providers-config.js`.
2.  **Feedback Loop**: MUST call `increaseTimeout()` on 429 and `decreaseTimeout()` on success to adapt to dynamic server loads.
3.  **One Limiter Per Domain**: Do not create multiple limiters for the same API key.

## 4. File Map
- `@core/api/rate-limiter.js`: Logic.
- `@core/config/data-providers-config.js`: Limits definition.
