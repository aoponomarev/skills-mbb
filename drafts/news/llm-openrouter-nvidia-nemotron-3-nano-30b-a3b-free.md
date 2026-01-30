---
id: llm-openrouter-nvidia-nemotron-3-nano-30b-a3b-free
title: "New LLM: NVIDIA: Nemotron 3 Nano 30B A3B (free)"
description_ru: "Новая модель NVIDIA: Nemotron 3 Nano 30B A3B (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# NVIDIA: Nemotron 3 Nano 30B A3B (free)

## Model Details
- **ID**: `nvidia/nemotron-3-nano-30b-a3b:free`
- **Provider**: OpenRouter Models
- **Context Length**: 256000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: nemotron-3-nano-30b-a3b:free
  provider: openai
  model: nvidia/nemotron-3-nano-30b-a3b:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
