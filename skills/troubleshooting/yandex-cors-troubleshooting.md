---
id: yandex-cors-troubleshooting
title: Troubleshooting: Yandex CORS
scope: skills-mbb
tags: [#troubleshooting, #yandex, #cors, #functions]
priority: medium
created_at: 2026-01-24
updated_at: 2026-02-01
---

# Troubleshooting: Yandex CORS

> **Context**: "Preflight request failed" errors.

## 1. Checklist
1.  **Public**: Function must be Public (invoke permission for `allUsers` or handled via Gateway).
2.  **OPTIONS**: Code MUST handle `httpMethod === 'OPTIONS'` and return 200 with headers.
3.  **Headers**:
    - `Access-Control-Allow-Origin: *`
    - `Access-Control-Allow-Headers: Content-Type, Authorization`

## 2. Debug
Use `curl` to test preflight:
```bash
curl -X OPTIONS https://.../func -i
```
