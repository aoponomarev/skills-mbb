---
title: core-systems-auto-coin-sets
tags:
  - "#mbb-spec"
  - "#core-systems"
  - "#automation"
dependencies: []
mcp_resource: true
updated_at: 2026-01-25
---

# core-systems-auto-coin-sets

## Scope
- Автоматическое формирование и обновление наборов монет.

## Key Components
- `core/utils/auto-coin-sets.js`
- `core/utils/draft-coin-set.js`

## Key Rules
- Наборы формируются по типам и правилам, без ручной правки в UI.
- Обновления происходят централизованно через утилиты.

## Workflow
1) Обновить правила формирования набора.
2) Перегенерировать наборы через утилиты.
3) Проверить консистентность результатов.

## References
- `core/utils/auto-coin-sets.js`
- `core/utils/draft-coin-set.js`
