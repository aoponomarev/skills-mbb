---
id: llm-openrouter-nousresearch-hermes-3-llama-3-1-405b-free
title: "New LLM: Nous: Hermes 3 405B Instruct (free)"
description_ru: "Новая модель Nous: Hermes 3 405B Instruct (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Nous: Hermes 3 405B Instruct (free)

## Model Details
- **ID**: `nousresearch/hermes-3-llama-3.1-405b:free`
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
- name: hermes-3-llama-3.1-405b:free
  provider: openai
  model: nousresearch/hermes-3-llama-3.1-405b:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
