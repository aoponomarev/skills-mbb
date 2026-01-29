---
id: cursor-release-vcli-jan-2026
title: "Cursor Release: vCLI Improvements (Jan 8, 2026)"
description_ru: "Обновление CLI Cursor: выбор моделей через 'agent models', управление правилами через '/rules' и MCP через '/mcp'."
scope: "External News: Cursor IDE Update"
tags: [#news, #cursor, #cli, #mcp]
priority: high
created_at: 2026-01-29
updated_at: 2026-01-29
utility_score: 9
comment_is_bot: true
user_comment: "Пока непонятно, насколько это усложнит развертывание Докера ввиду того что Cursor CLI требует особой среды пол Linux (кажется так)"
impact_analysis: "Внедрение нового vCLI значительно ускорит управление MCP-серверами в MBB. Команда '/mcp enable/disable' позволит агенту самостоятельно активировать нужные инструменты без ручной правки JSON. Это критический шаг к полной автономности инфраструктуры.

Для агентов это открывает возможность самостоятельно управлять своими рабочими процессами, включая активацию и деактивацию необходимых инструментов через MCP. Это устранит необходимость ручного вмешательства и позволит агентам более эффективно выполнять задачи, что значительно повысит общую производительность и надежность системы. В результате проект MBB станет более устойчивым и переносимым, что сделает его полностью автономным и неуязвимым к внешним вмешательствам."
---

# Cursor Release: vCLI Improvements (Jan 8, 2026)

## Overview
The latest version of Cursor (Jan 2026) introduces major CLI improvements, specifically focused on the `agent` command and MCP management.

## Key Features
- **Model Selection**: Use `agent models` to switch between models via CLI.
- **Rules Management**: New `/rules` command to manage project-level instructions.
- **MCP Management**: `/mcp enable` and `/mcp disable` for easier server management.
- **Tab Naming**: Automatic tab naming based on chat context.

## Architectural Utility
For the MBB project, the most valuable addition is the **MCP management via CLI**. This allows our `infra-manager.js` or even the agent itself to enable/disable specific MCP gateways dynamically.

## Scoring
- **Implementation Priority**: 9/10
- **Complexity**: Low (just a binary update)
- **Impact**: High (automation of MCP setup)
