---
title: "API Enhancement: Dynamic Agent Filtering"
description: "Upgrade GET /api/agents to support query params (tags, min_rating) for flexible swarm selection."
context: "Currently, n8n fetches all agents and filters in JS. We need the backend to handle logic like ?tag=coding&count=3 to make the swarm universal."
priority_score: 85
project_type: Task
status: draft
created_at: 2026-02-03T12:00:00Z
tags: [backend, api, optimization]
---

# Implementation Plan

To make the "Get Agents" node in n8n truly universal, the backend must handle selection logic.

## Objectives
- Update `server.js` endpoint `GET /api/agents`.
- Add URL query parameter support:
  - `?tag=scout|commander|coding|fast`
  - `?min_rating=4.0`
  - `?status=active` (default)
  - `?limit=3`
- Ensure `AGENT_REGISTRY.json` is queried efficiently.

## Example Usage in n8n
Instead of a complex JS Function node, n8n will simply make an HTTP Request:
`GET http://host.docker.internal:3000/api/agents?tag=coding&limit=3`

## Definition of Done
- Endpoint returns filtered JSON list.
- Fallback to returning all agents if no params provided.
- Frontend UI continues to work (may need minor adjustment if it expects a specific structure).
