---
id: llm-openrouter-openrouter-bodybuilder
title: "New LLM: Body Builder (beta)"
description_ru: "Новая модель Body Builder (beta) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Body Builder (beta)

## Model Details
- **ID**: `openrouter/bodybuilder`
- **Provider**: OpenRouter Models
- **Context Length**: 128000
- **Free**: No
- **Pricing**: $-1/1K prompt, $-1/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: bodybuilder
  provider: openai
  model: openrouter/bodybuilder
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
