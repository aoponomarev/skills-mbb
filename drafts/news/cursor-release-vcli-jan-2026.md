---
id: cursor-release-vcli-jan-2026
title: "Cursor Release: vCLI Improvements (Jan 8, 2026)"
description_ru: "Обновление CLI Cursor: выбор моделей через 'agent models', управление правилами через '/rules' и MCP через '/mcp'."
scope: "External News: Cursor IDE Update"
tags: [#news, #cursor, #cli, #mcp]
priority: high
created_at: 2026-01-29
updated_at: 2026-01-29
impact_analysis: "Внедрение нового vCLI значительно ускорит управление MCP-серверами в MBB. Команда '/mcp enable/disable' позволит агенту самостоятельно активировать нужные инструменты без ручной правки JSON. Это критический шаг к полной автономности инфраструктуры."
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
