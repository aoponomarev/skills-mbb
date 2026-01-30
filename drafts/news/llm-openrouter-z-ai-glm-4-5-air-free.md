---
id: llm-openrouter-z-ai-glm-4-5-air-free
title: "New LLM: Z.AI: GLM 4.5 Air (free)"
description_ru: "Новая модель Z.AI: GLM 4.5 Air (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Z.AI: GLM 4.5 Air (free)

## Model Details
- **ID**: `z-ai/glm-4.5-air:free`
- **Provider**: OpenRouter Models
- **Context Length**: 131072
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: glm-4.5-air:free
  provider: openai
  model: z-ai/glm-4.5-air:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
