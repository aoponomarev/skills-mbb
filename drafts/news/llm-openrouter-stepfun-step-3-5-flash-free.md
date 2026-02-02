---
id: llm-openrouter-stepfun-step-3-5-flash-free
title: "New LLM: StepFun: Step 3.5 Flash (free)"
description_ru: "Новая модель StepFun: Step 3.5 Flash (free) обнаружена в OpenRouter Models"
created_at: 2026-02-02
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# StepFun: Step 3.5 Flash (free)

## Model Details
- **ID**: `stepfun/step-3.5-flash:free`
- **Provider**: OpenRouter Models
- **Context Length**: 256000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update logs/llm-models-registry.md

## API Configuration
```yaml
- name: step-3.5-flash:free
  provider: openai
  model: stepfun/step-3.5-flash:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
