---
id: llm-groq-llama-3-1-8b-instant
title: "New LLM: llama-3.1-8b-instant"
description_ru: "Новая модель llama-3.1-8b-instant обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# llama-3.1-8b-instant

## Model Details
- **ID**: `llama-3.1-8b-instant`
- **Provider**: Groq Models
- **Context Length**: Unknown
- **Free**: Yes


## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: llama-3.1-8b-instant
  provider: openai
  model: llama-3.1-8b-instant
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
