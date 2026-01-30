---
id: llm-groq-meta-llama-llama-4-scout-17b-16e-instruct
title: "New LLM: meta-llama/llama-4-scout-17b-16e-instruct"
description_ru: "Новая модель meta-llama/llama-4-scout-17b-16e-instruct обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# meta-llama/llama-4-scout-17b-16e-instruct

## Model Details
- **ID**: `meta-llama/llama-4-scout-17b-16e-instruct`
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
- name: llama-4-scout-17b-16e-instruct
  provider: openai
  model: meta-llama/llama-4-scout-17b-16e-instruct
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
