---
id: llm-groq-moonshotai-kimi-k2-instruct
title: "New LLM: moonshotai/kimi-k2-instruct"
description_ru: "Новая модель moonshotai/kimi-k2-instruct обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# moonshotai/kimi-k2-instruct

## Model Details
- **ID**: `moonshotai/kimi-k2-instruct`
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
- name: kimi-k2-instruct
  provider: openai
  model: moonshotai/kimi-k2-instruct
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
