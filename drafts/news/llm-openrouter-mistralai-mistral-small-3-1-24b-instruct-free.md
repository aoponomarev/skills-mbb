---
id: llm-openrouter-mistralai-mistral-small-3-1-24b-instruct-free
title: "New LLM: Mistral: Mistral Small 3.1 24B (free)"
description_ru: "Новая модель Mistral: Mistral Small 3.1 24B (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Mistral: Mistral Small 3.1 24B (free)

## Model Details
- **ID**: `mistralai/mistral-small-3.1-24b-instruct:free`
- **Provider**: OpenRouter Models
- **Context Length**: 128000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: mistral-small-3.1-24b-instruct:free
  provider: openai
  model: mistralai/mistral-small-3.1-24b-instruct:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
