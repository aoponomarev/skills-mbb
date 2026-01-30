---
id: llm-openrouter-nvidia-nemotron-nano-12b-v2-vl-free
title: "New LLM: NVIDIA: Nemotron Nano 12B 2 VL (free)"
description_ru: "Новая модель NVIDIA: Nemotron Nano 12B 2 VL (free) обнаружена в OpenRouter Models"
created_at: 2026-01-30
tags: [#news, #llm, #openrouter, #free]
utility_score: 0
project_type: LLM
comment_is_bot: true
user_comment: "Бот обнаружил новую модель. Требуется анализ для включения в config.yaml"
---

# NVIDIA: Nemotron Nano 12B 2 VL (free)

## Model Details
- **ID**: `nvidia/nemotron-nano-12b-v2-vl:free`
- **Provider**: OpenRouter Models
- **Context Length**: 128000
- **Free**: Yes
- **Pricing**: $0/1K prompt, $0/1K completion

## Analysis Required
- [ ] Test model quality for coding tasks
- [ ] Determine appropriate Tier (1-4)
- [ ] Add to config.yaml if suitable
- [ ] Update MODELS_REGISTRY.md

## API Configuration
```yaml
- name: nemotron-nano-12b-v2-vl:free
  provider: openai
  model: nvidia/nemotron-nano-12b-v2-vl:free
  apiKey: ${OPENROUTER_API_KEY}
  apiBase: https://openrouter.ai/api/v1
  roles:
    - chat
```
