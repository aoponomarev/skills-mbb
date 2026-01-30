---
id: llm-openrouter-moonshotai-kimi-k2-free
title: "New LLM: MoonshotAI: Kimi K2 0711 (free)"
description_ru: "Новая модель MoonshotAI: Kimi K2 0711 (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# MoonshotAI: Kimi K2 0711 (free)

## Model Details
- **ID**: `moonshotai/kimi-k2:free`
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
- name: kimi-k2:free
  provider: openai
  model: moonshotai/kimi-k2:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
