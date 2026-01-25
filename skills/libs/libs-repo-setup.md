---
title: libs-repo-setup
tags: [#mbb-spec, #libs, #github-pages, #cdn]
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---

# libs-repo-setup

## Scope
- Настройка отдельного репозитория `libs` для GitHub Pages CDN.
- Структура папок и загрузка библиотек.

## When to Use
- Когда обновляете/добавляете внешние библиотеки для MBB.
- Когда нужно поднять/восстановить libs‑репозиторий.

## Key Rules
- Репозиторий публичный и развёрнут через GitHub Pages.
- Структура `lib-name/version/file.js`.
- В проекте загрузка идёт через `core/lib-loader.js` с fallback на внешние CDN.

## Workflow
1) Создать репозиторий `https://github.com/aoponomarev/libs`.
2) Развернуть структуру папок по версиям.
3) Загрузить файлы вручную или скриптом `download-libs.sh`.
4) Включить GitHub Pages: branch `main`, folder `/`.
5) Проверить доступность через `curl`.

## References
- `core/lib-loader.js`
