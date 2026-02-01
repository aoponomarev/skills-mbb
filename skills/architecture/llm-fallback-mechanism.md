---
id: llm-fallback-mechanism
title: LLM Fallback Mechanism
description_ru: Механизм обработки ошибок и отказов в LLM, обеспечивающий плавное переключение на альтернативные методы при неудачах основного моделирования.
scope: architecture, reliability
tags: [architecture, error-handling, reliability, LLM]
priority: high
created_at: 2026-02-01
updated_at: 2026-02-01
---

# LLM Fallback Mechanism

## Overview
This skill defines a robust fallback mechanism for Large Language Models (LLMs) to handle various failure scenarios including:
- Generation timeouts
- Content filtering failures
- Model unavailability
- Quality degradation
- Context window limitations

The mechanism ensures graceful degradation of service while maintaining user experience.

## Steps

### 1. Error Detection
- Implement real-time monitoring of LLM responses with these checks:
  ```javascript
  function detectFailure(response) {
      return (
          response.timeout ||
          response.errorCode !== 0 ||
          response.contentFiltered ||
          response.qualityScore < threshold ||
          response.length > maxLength
      );
  }
  ```

### 2. Fallback Strategy Selection
- Use a tiered fallback approach:
  ```javascript
  function selectFallback(error) {
      if (error.timeout) return "retry_with_longer_timeout";
      if (error.rateLimit) return "switch_to_next_tier_provider";
      if (error.contentFiltered) return "rephrase_request";
      return "default_fallback";
  }
  ```

### 3. Fallback Execution
- Implement specific fallback handlers based on the priority chain (e.g., Mistral -> Groq -> OpenRouter).
- Track health metrics (failures, last success) for each model to optimize selection.

### 4. Response Synthesis
- Combine fallback results with original response when appropriate or choose the best available output.

## Validation
### Success Criteria
1. Fallback mechanism triggers only when necessary (false positive rate < 1%).
2. Fallback responses maintain acceptable quality (at least 70% of original).
3. System availability improves significantly (targeting 99.9% uptime).

### Monitoring
- Implement metrics for tracking:
  - `fallback_trigger_rate`
  - `fallback_success_rate`
  - `latency_with_fallback`
  - `provider_availability_heatmap`
