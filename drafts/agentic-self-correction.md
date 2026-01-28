---
id: agentic-self-correction
title: Agentic Self-Correction & Autonomy
description_ru: Стратегия автономного выживания агента: вместо того чтобы останавливаться при любой ошибке (сбой API, отсутствие файла, неверный путь), агент анализирует причину сбоя, переосмысливает свой план и ищет альтернативные способы достичь цели. Это превращает систему из хрупкого набора инструкций в самовосстанавливающийся механизм, способный работать в нестабильных условиях.
scope: Guidelines for agents to handle errors autonomously by rethinking and adapting instead of failing.
tags: [#architecture, #process, #agentic]
priority: high
created_at: 2026-01-28
updated_at: 2026-01-28
---

# Agentic Self-Correction & Autonomy

> "N8N: Error = Everything Broken. Agentic: Error = Agent Finishes Itself."

## Scope

This skill is applied when an agent encounters an error, a broken workflow, or an unexpected state. Instead of stopping or asking for help immediately, the agent should attempt to self-correct.

## When to Use

- When a script fails with a non-critical error.
- When an API returns an unexpected response.
- When a file operation fails due to missing directories.
- When the path to a goal is blocked.

## Overview

In traditional hard-coded automation (like n8n), an error in one node breaks the entire sequence. In an **agentic paradigm**, an error is simply a piece of data that the agent uses to refine its plan.

## Guidelines

1. **Rethink on Failure**: If a tool call fails, analyze the error message. Is it a syntax error, a path error, or a permission issue?
2. **Dynamic Adaptation**: If path `A` is blocked, search for path `B`. If a library is missing, try to install it or use an alternative.
3. **Sandbox Recovery**: If running in a sandbox, use the available tools to explore the environment and fix the state before retrying.
4. **Autonomous Goal-Seeking**: Focus on the *goal*, not the *steps*. If the predefined steps fail, generate a new plan.

## Examples

### Scenario: Missing Directory
- **Action**: `mkdir` failed because parent doesn't exist.
- **Self-Correction**: Run `ls` on parent, create parent recursively, then retry.

### Scenario: Deprecated API
- **Action**: Tool returned "method not found".
- **Self-Correction**: Search documentation or use `Glob` to find the new method name.

## Validation

- The agent reaches the final goal despite intermediate failures.
- Logs show "Error encountered -> Rethinking -> New plan initiated".
