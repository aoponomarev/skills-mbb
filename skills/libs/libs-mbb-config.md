# libs-mbb-config

> Источник: `docs/doc-lib-vue.md` (схема хранения и загрузка)

## Структура libs

Repo: `https://github.com/aoponomarev/libs`

```
libs/
├── vue/3.4.0/vue.global.js
├── chartjs/4.4.0/chart.umd.js
├── numeral/2.0.6/numeral.min.js
├── vuedraggable/4.1.0/vuedraggable.umd.js
```

## Источники загрузки (приоритет)

1. GitHub Pages CDN: `https://aoponomarev.github.io/libs/`
2. jsdelivr / cdnjs
3. локальные файлы `./libs/`

## Механизм загрузки

Через `core/lib-loader.js` с автоматическим fallback.

Пример:
```javascript
await window.libLoader.load('vue', '3.4.0');
```
