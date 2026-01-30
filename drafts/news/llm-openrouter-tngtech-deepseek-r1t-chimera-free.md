---
id: llm-openrouter-tngtech-deepseek-r1t-chimera-free
title: "New LLM: TNG: DeepSeek R1T Chimera (free)"
description_ru: "Новая модель TNG: DeepSeek R1T Chimera (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# TNG: DeepSeek R1T Chimera (free)

## Model Details
- **ID**: `tngtech/deepseek-r1t-chimera:free`
- **Provider**: OpenRouter Models
- **Context Length**: 163840
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: deepseek-r1t-chimera:free
  provider: openai
  model: tngtech/deepseek-r1t-chimera:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
