---
id: llm-groq-allam-2-7b
title: "New LLM: allam-2-7b"
description_ru: "Новая модель allam-2-7b обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# allam-2-7b

## Model Details
- **ID**: `allam-2-7b`
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
- name: allam-2-7b
  provider: openai
  model: allam-2-7b
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
