---
id: llm-openrouter-openrouter-free
title: "New LLM: Free Router"
description_ru: "Новая модель Free Router обнаружена в OpenRouter Models"
created_at: 2026-02-02
tags: [#news, #llm, #openrouter]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Free Router

## Model Details
- **ID**: `openrouter/free`
- **Provider**: OpenRouter Models
- **Context Length**: 200000
- **Free**: No
- **Pricing**: $-1/1K prompt, $-1/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update logs/llm-models-registry.md

## API Configuration
```yaml
- name: free
  provider: openai
  model: openrouter/free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
