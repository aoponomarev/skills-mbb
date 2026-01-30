---
id: llm-groq-qwen-qwen3-32b
title: "New LLM: qwen/qwen3-32b"
description_ru: "Новая модель qwen/qwen3-32b обнаружена в Groq Models"
created_at: 2026-01-30
tags: [#news, #llm, #groq, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# qwen/qwen3-32b

## Model Details
- **ID**: `qwen/qwen3-32b`
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
- name: qwen3-32b
  provider: openai
  model: qwen/qwen3-32b
  apiKey: ${GROQ_API_KEY}
  apiBase: https://api.groq.com/openai/v1
  roles:
    - chat
```
