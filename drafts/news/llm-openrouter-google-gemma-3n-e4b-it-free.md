---
id: llm-openrouter-google-gemma-3n-e4b-it-free
title: "New LLM: Google: Gemma 3n 4B (free)"
description_ru: "Новая модель Google: Gemma 3n 4B (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Google: Gemma 3n 4B (free)

## Model Details
- **ID**: `google/gemma-3n-e4b-it:free`
- **Provider**: OpenRouter Models
- **Context Length**: 8192
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: gemma-3n-e4b-it:free
  provider: openai
  model: google/gemma-3n-e4b-it:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
