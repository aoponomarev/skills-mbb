---
id: llm-groq-canopylabs-orpheus-v1-english
title: "New LLM: canopylabs/orpheus-v1-english"
description_ru: "Новая модель canopylabs/orpheus-v1-english обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# canopylabs/orpheus-v1-english

## Model Details
- **ID**: `canopylabs/orpheus-v1-english`
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
- name: orpheus-v1-english
  provider: openai
  model: canopylabs/orpheus-v1-english
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
