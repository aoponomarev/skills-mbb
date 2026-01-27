---
title: libs-mbb-auto-activation
tags:
  - "#mbb-spec"
  - "#libs"
dependencies: []
mcp_resource: true
updated_at: 2026-01-24
---
## Scope

- Libs Mbb Auto Activation functionality and configuration.

## When to Use

- При необходимости работы с данным компонентом или функционалом.

# libs-mbb-auto-activation

> Источник: `docs/doc-lib-github.md` (автоматическая активация)

## Триггеры

- добавление библиотеки в `core/lib-loader.js` (`LIB_SOURCES`)
- изменение версии в `LIB_SOURCES`
- библиотека отсутствует в `libs`

## Шаги автоматической активации

1. Выявить новую/измененную библиотеку и версию.
2. Проверить наличие файла в `libs/<name>/<version>/`.
3. Если файла нет — загрузить UMD‑сборку из CDN.
4. Обновить `docs/doc-lib-vue.md` (версия/источники).
5. Уведомить пользователя о необходимости коммита в `libs`.

## Ограничения

- Коммиты только по явной команде пользователя.
- Если UMD отсутствует — уведомить пользователя.
