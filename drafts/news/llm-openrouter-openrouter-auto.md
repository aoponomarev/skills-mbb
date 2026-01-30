---
id: llm-openrouter-openrouter-auto
title: "New LLM: Auto Router"
description_ru: "Новая модель Auto Router обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Auto Router

## Model Details
- **ID**: `openrouter/auto`
- **Provider**: OpenRouter Models
- **Context Length**: 2000000
- **Free**: No
- **Pricing**: $-1/1K prompt, $-1/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: auto
  provider: openai
  model: openrouter/auto
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
