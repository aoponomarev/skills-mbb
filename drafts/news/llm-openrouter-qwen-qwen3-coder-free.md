---
id: llm-openrouter-qwen-qwen3-coder-free
title: "New LLM: Qwen: Qwen3 Coder 480B A35B (free)"
description_ru: "Новая модель Qwen: Qwen3 Coder 480B A35B (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Qwen: Qwen3 Coder 480B A35B (free)

## Model Details
- **ID**: `qwen/qwen3-coder:free`
- **Provider**: OpenRouter Models
- **Context Length**: 262000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: qwen3-coder:free
  provider: openai
  model: qwen/qwen3-coder:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
