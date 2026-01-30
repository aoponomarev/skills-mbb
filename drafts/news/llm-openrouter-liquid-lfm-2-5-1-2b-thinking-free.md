---
id: llm-openrouter-liquid-lfm-2-5-1-2b-thinking-free
title: "New LLM: LiquidAI: LFM2.5-1.2B-Thinking (free)"
description_ru: "Новая модель LiquidAI: LFM2.5-1.2B-Thinking (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# LiquidAI: LFM2.5-1.2B-Thinking (free)

## Model Details
- **ID**: `liquid/lfm-2.5-1.2b-thinking:free`
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
- name: lfm-2.5-1.2b-thinking:free
  provider: openai
  model: liquid/lfm-2.5-1.2b-thinking:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
