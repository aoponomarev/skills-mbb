---
id: llm-openrouter-openai-gpt-oss-120b-free
title: "New LLM: OpenAI: gpt-oss-120b (free)"
description_ru: "Новая модель OpenAI: gpt-oss-120b (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# OpenAI: gpt-oss-120b (free)

## Model Details
- **ID**: `openai/gpt-oss-120b:free`
- **Provider**: OpenRouter Models
- **Context Length**: 131072
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: gpt-oss-120b:free
  provider: openai
  model: openai/gpt-oss-120b:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
