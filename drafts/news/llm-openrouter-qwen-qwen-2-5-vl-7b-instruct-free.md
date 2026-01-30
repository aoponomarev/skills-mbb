---
id: llm-openrouter-qwen-qwen-2-5-vl-7b-instruct-free
title: "New LLM: Qwen: Qwen2.5-VL 7B Instruct (free)"
description_ru: "Новая модель Qwen: Qwen2.5-VL 7B Instruct (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Qwen: Qwen2.5-VL 7B Instruct (free)

## Model Details
- **ID**: `qwen/qwen-2.5-vl-7b-instruct:free`
- **Provider**: OpenRouter Models
- **Context Length**: 32768
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: qwen-2.5-vl-7b-instruct:free
  provider: openai
  model: qwen/qwen-2.5-vl-7b-instruct:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
