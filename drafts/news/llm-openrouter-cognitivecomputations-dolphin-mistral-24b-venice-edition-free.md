---
id: llm-openrouter-cognitivecomputations-dolphin-mistral-24b-venice-edition-free
title: "New LLM: Venice: Uncensored (free)"
description_ru: "Новая модель Venice: Uncensored (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Venice: Uncensored (free)

## Model Details
- **ID**: `cognitivecomputations/dolphin-mistral-24b-venice-edition:free`
- **Provider**: OpenRouter Models
- **Context Length**: 32768
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: dolphin-mistral-24b-venice-edition:free
  provider: openai
  model: cognitivecomputations/dolphin-mistral-24b-venice-edition:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
