---
id: autonomous-skill-synthesis
title: Autonomous Skill Synthesis Protocol
description_ru: Механизм эволюции интеллекта: в конце каждой задачи агент обязан проанализировать свой опыт, выделить новые удачные решения или архитектурные находки и самостоятельно предложить их в качестве постоянных «навыков». Таким образом, база знаний проекта растет органически в процессе работы, а не только по команде человека.
scope: Guidelines for agents to proactively expand their own knowledge base by distilling complex logic into reusable skills.
tags: [#architecture, #process, #agentic, #meta]
priority: high
created_at: 2026-01-28
updated_at: 2026-01-29
---

# Autonomous Skill Synthesis Protocol

> "Skill Creator: the agent creates its own tools! - The agent building its own capabilities."

## Scope

This skill is a "Meta-Skill" that governs how agents should behave at the end of a task. It defines the protocol for identifying new "capabilities" or "playbooks" that were born during the current session.

## When to Use

- After completing any significant task that involved architectural decisions.
- When you find yourself repeating a multi-step logic across different files.
- When you've solved a complex problem that wasn't previously documented in the Skills base.
- During the "Post-Action Analysis" phase of a task.

## Overview

In the **agentic paradigm**, an agent is not just a worker but a **contributor to its own evolutionary code**. Instead of waiting for a human to document a process, the agent distills its successful logic into a "Genetic Code" (a Skill) and proposes it for the knowledge base.

## Guidelines

1. **Post-Action Analysis**: After finishing a task, take 10 seconds to look at the "Genetic Code" of your solution. Is there a reusable pattern?
2. **Genetic Code Distillation**: If the logic is reusable, simplify it. Remove project-specific data and extract the *methodology*.
3. **Proactive Proposal**: Use the `propose_skill` tool or write directly to the `BACKLOG.md` to register the need for this new skill.
4. **Self-Documentation**: Don't just save code; save the "Why" and "When to Use".

## Protocol Steps

1. **Identify**: "Wait, I just spent 20 minutes figuring out how to fix Docker port conflicts. This should be a skill."
2. **Abstract**: "The core issue was Windows reserving ports. The solution was checking `netstat` and changing `docker-compose.yml`."
3. **Draft**: Create a entry in `BACKLOG.md` with:
   - `action=create`
   - `title="Handling Windows Port Conflicts in Docker"`
   - `scope="Guidelines for identifying and resolving port reservations by OS processes."`
4. **Register**: Notify the user that a new capability has been proposed.

## Validation

- The agent context remains lean because logic is offloaded to the Skills base.
- Repetitive errors decrease over time as agents "remember" solutions through the Knowledge Base.
- The `BACKLOG.md` grows automatically with relevant technical insights.
