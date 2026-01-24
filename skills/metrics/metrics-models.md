# metrics-models

> Источник: `docs/doc-metrics.md` (архитектура моделей)

## Архитектура

- **BaseModelCalculator** — `core/metrics/base-model-calculator.js`
- **ModelManager** — `core/metrics/model-manager.js`
- **ModelsConfig** — `core/config/models-config.js`
- Реализации модели «Медиана» — `mm/Median/AIR/...`

## Принцип расширяемости

Новые модели добавляются без изменения кода приложения: регистрация в ModelManager и конфиге.
