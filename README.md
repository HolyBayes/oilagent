# OilAgent

[![CI](https://github.com/HolyBayes/oilagent-core/actions/workflows/ci.yml/badge.svg)](https://github.com/HolyBayes/oilagent-core/actions/workflows/ci.yml)

Нефтегазовый аналитик на Ouroboros и LangGraph: ответы по отчётам OPEC/EIA и web,
прогноз Brent, создание кандидатов на улучшение скиллов и Python-методов.

| Submodule | Содержимое |
|---|---|
| [core](https://github.com/HolyBayes/oilagent-core) | MCP, LangGraph, executor, Docker и тесты |
| [evolution](https://github.com/HolyBayes/oilagent-evolution) | Четыре самоулучшаемых скилла, шесть Python-методов и policies |

## Запуск

Нужны Docker с Compose v2, Python 3.11+ и интернет. При первом запуске скачиваются PDF и веса.

```bash
git clone --recurse-submodules https://github.com/HolyBayes/oilagent.git
cd oilagent/core
cp .env.example .env
chmod 600 .env
```

Заполните `GIGACHAT_CREDENTIALS` и `GIGACHAT_SCOPE` в `.env`;
при необходимости укажите `GIGACHAT_CA_BUNDLE_FILE`. По умолчанию — Lite (`GigaChat-2`).

```bash
./run.sh -d
```

Откройте **http://127.0.0.1:8765**. В Skills выполните Review и включите
`oilgas_analyst`, `oil_forecast`, `report_rag`, `web_research`. Review расходует токены.

Попробуйте: «Что EIA в октябре 2025 прогнозировало для Brent в первом полугодии 2026?»
или «Рассчитай новый прогноз Brent на 3 месяца с интервалами 80%».

Остановить UI: `./compose.sh stop ouroboros`. Весь стек: `./compose.sh down`.
Данные сохраняются.

## Документация

- [Настройки и выпуск изменений](https://github.com/HolyBayes/oilagent-core#конфигурация)
- [Архитектура и TODO](https://github.com/HolyBayes/oilagent-core/blob/main/ARCHITECTURE.md)
- [Результаты проверок](https://github.com/HolyBayes/oilagent-core/blob/main/docs/VALIDATION.md)
- [Пять диалогов](https://github.com/HolyBayes/oilagent-core/blob/main/examples/demo_dialogues.md)

Проверка с нуля прошла: новые данные, четыре первичных review и пять задач через UI API.
Это прототип: корпус — два отчёта, прогноз — только Brent на 1–6 месяцев,
OpenAI/local живьём не проверены. Кандидаты из чата требуют проверки и активации оператором.

Submodules фиксируют совместимые commits. Коммит внутри `evolution` не обновляет сборку:
новый указатель сохраняет оператор. `git submodule update --init --recursive` возвращает
зафиксированные версии; перед обновлением сохраните локальные изменения.
