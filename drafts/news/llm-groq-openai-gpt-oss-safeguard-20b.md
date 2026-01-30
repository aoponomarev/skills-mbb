---
id: llm-groq-openai-gpt-oss-safeguard-20b
title: "New LLM: openai/gpt-oss-safeguard-20b"
description_ru: "Новая модель openai/gpt-oss-safeguard-20b обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# openai/gpt-oss-safeguard-20b

## Model Details
- **ID**: `openai/gpt-oss-safeguard-20b`
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
- name: gpt-oss-safeguard-20b
  provider: openai
  model: openai/gpt-oss-safeguard-20b
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
