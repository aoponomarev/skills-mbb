---
id: process-news-intelligence
title: "Протокол мониторинга и анализа внешних новостей (News Intelligence)"
description_ru: "Алгоритм автономного сбора обновлений платформ (Cursor, Continue, Docker, n8n), оценки их влияния на MBB и автоматического расширения базы источников."
scope: "Автоматизация мониторинга технологического стека MBB для проактивной модернизации навыков."
tags: [#process, #news, #automation, #infrastructure]
priority: medium
created_at: 2026-01-29
updated_at: 2026-01-29
dependencies: ["process-infrastructure-maintenance", "process-future-skill-impact-analysis"]
---

# Протокол мониторинга и анализа внешних новостей (News Intelligence)

> **Skill ID:** `process-news-intelligence`

## Обзор

Этот протокол управляет тем, как проект MBB узнает о новых возможностях своих базовых инструментов (Cursor, Continue, n8n и др.) и превращает их в проекты новых навыков.

## 1. Сбор и контроль источников

Мониторинг осуществляется скриптом `scripts/news-watcher.js`.

### Правила проверки (Availability Control):
- **Периодичность**: Каждые 12-24 часа.
- **Действие**: Проверка доступности (HTTP 200).
- **Self-Healing**: Если источник недоступен (например, URL GitHub Releases изменился), агент должен попытаться найти актуальный путь через веб-поиск или использовать fallback (основную страницу репозитория).

### Авто-обнаружение (Auto-Discovery):
При добавлении нового ПО или сервиса в `INFRASTRUCTURE_CONFIG.yaml`, агент должен:
1. Идентифицировать название инструмента.
2. Проверить, есть ли он в `events/NEWS_SOURCES.json`.
3. Если нет — найти официальную страницу обновлений (Changelog/Releases) и добавить в реестр.

## 2. Анализ и Скорринг (Utility Scoring)

После получения новости, агент выполняет оценку:
- **Direct Utility**: Решает ли это текущую проблему в MBB?
- **Agent Boost**: Помогает ли это Cursor/Continue стать умнее?
- **Implementation Cost**: Насколько сложно это внедрить?

**Формула скоринга (0-10):**
- **8-10**: Критическое улучшение (нужно внедрять немедленно).
- **5-7**: Полезное дополнение (отложить во Future).
- **0-4**: Низкий приоритет или чисто декоративное изменение.

## 3. Генерация проектов (News Tab)

На основании анализа создается файл в `skills-mbb/drafts/news/`.
Обязательные поля в YAML:
- `utility_score`: Оценка бота.
- `impact_analysis`: Текст анализа на русском языке.
- `user_comment`: Поле для обратной связи от человека.
- `comment_is_bot`: Флаг (true/false). Если true — комментарий отображается полупрозрачным (черновик бота).

## 4. Жизненный цикл

1. **News** (Проект) → **Analyze** (Оценка ИИ) → **Comment** (Мнение пользователя).
   *   При анализе бот может создать `user_comment` с флагом `comment_is_bot: true`.
   *   При сохранении комментария пользователем флаг `comment_is_bot` сбрасывается в `false`.
2. Если `SCORE > 7` и одобрено → Перенос в **Publish** (Drafts).
   *   **ВАЖНО**: При переносе бот-комментарии (`comment_is_bot: true`) должны игнорироваться.
3. Если требует времени → Перенос во **Future**.

## Реестр источников

Хранится в `events/NEWS_SOURCES.json`.
Примеры:
- **Cursor**: `https://cursor.com/changelog`
- **Continue**: `https://github.com/continuedev/continue/releases`
- **n8n**: `https://github.com/n8n-io/n8n/releases`
