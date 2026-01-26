# Skills Drafts
Сюда n8n складывает автоматически сгенерированные черновики навыков для последующей проверки.

## Statuses
- `status=draft`: новый Skill из `action=create`
- `status=update`: обновленный Skill из `action=update`
- `status=move-ready`: инструкция для перемещения `action=move` (без генерации контента)
- `status=merge`: объединенный Skill из `action=merge`/`split`
