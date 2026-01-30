---
id: llm-openrouter-arcee-ai-trinity-large-preview-free
title: "New LLM: Arcee AI: Trinity Large Preview (free)"
description_ru: "Новая модель Arcee AI: Trinity Large Preview (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# Arcee AI: Trinity Large Preview (free)

## Model Details
- **ID**: `arcee-ai/trinity-large-preview:free`
- **Provider**: OpenRouter Models
- **Context Length**: 131000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: trinity-large-preview:free
  provider: openai
  model: arcee-ai/trinity-large-preview:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
