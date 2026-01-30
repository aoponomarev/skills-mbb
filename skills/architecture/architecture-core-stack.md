---
id: architecture-core-stack
title: "Ядро архитектуры и технологический стек MBB"
description_ru: "Актуальное описание технологического стека проекта MBB, включая Docker, MCP, Node.js и фронтенд."
scope: "Фундаментальный стек технологий, среда исполнения и принципы взаимодействия компонентов."
tags: [#architecture, #stack, #docker, #mcp, #frontend]
priority: emergency
created_at: 2026-01-28
updated_at: 2026-01-28
---

# Ядро архитектуры и технологический стек MBB (ааиисс)

**Команда активации:** `ааиисс` (Архитектура / ИнфраСтруктура). При ее использовании агент проводит полный анализ архитектурных связей, инфраструктуры Docker, MCP и облачных сервисов.

## Среда исполнения (Hybrid Environment)

Проект MBB использует гибридную среду исполнения для обеспечения максимальной автономности и гибкости:

1.  **Frontend**: Статическое приложение (GitHub Pages или `file://`), работающее в браузере.
2.  **Local Backend (Docker)**: Набор контейнеров для обслуживания ИИ-инструментария:
    *   `continue-cli`: Контейнер с Node.js Express сервером (`continue-wrapper`) и Continue CLI.
    *   `skills-mcp`: MCP-сервер для доступа к базе знаний проекта.
3.  **Cloud (Cloudflare & Yandex)**: Внешние воркеры и функции для проксирования API (обход CORS на `file://`) и тяжелых вычислений.

## Технологический Стек

### Backend & AI Infrastructure
*   **Runtime**: Node.js (v20+) внутри Docker.
*   **API Wrapper**: Express.js сервер для проброса команд CLI в HTTP.
*   **Knowledge Management**: MCP (Model Context Protocol) сервер для интеграции Skills в контекст ИИ-агентов.
*   **AI Engines**: Mistral Small (Cloud), Ollama Qwen (Local Fallback).

### Frontend (MBB Core)
*   **Vanilla JS + Custom Components**: Проект отошел от Vue.js в сторону кастомной компонентной модели (см. `shared/components/`).
*   **Styling**: Bootstrap 5 (CSS/JS) — приоритет утилитных классов.
*   **Storage**: Браузерное `localStorage` с системой версионированного кэширования (см. `cache-versioning`).
*   **Proxying**: Обязательное проксирование внешних API через Cloudflare Worker при работе на `file://`.

### Data & Events
*   **JSON-based SSOT**: Все очереди (кандидаты, идеи, реестры сканирования) хранятся в `.json` файлах в папке `events/`.
*   **Logging**: Единый лог событий `logs/skills-events.log`.

## Критические Принципы
1.  **SSOT (Single Source of Truth)**: Конфиги в `core/config/` — единственный источник правды для UI и логики.
2.  **Secrets Hygiene**: Секреты только в `.env` (не попадают в Docker/Git).
3.  **Skills-First**: Любое архитектурное решение должно быть согласовано с базой Skills.

## Документация
*   `docker-compose.yml` — описание инфраструктуры.
*   `mcp/skills-mcp/server.js` — реализация MCP.
*   `docs/CONTINUE_SKILLS_INTEGRATION.md` — детали интеграции Continue.
