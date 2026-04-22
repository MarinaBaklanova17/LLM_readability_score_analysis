# LLM_readability_score_analysis

Статистическая оценка читаемости текстов, сгенерированных большими языковыми моделями, в сравнении с классическими текстами, написанными людьми.

Является дополнением к [Story-generation-process](https://github.com/MarinaBaklanova17/Story-generation-process). Часть магистерской исследовательской работы в Whitireia New Zealand (2025).

## Мотивация

С ростом объёма контента, генерируемого LLM, возможность подбирать вывод под целевую аудиторию — в данном случае, детей — зависит от измеримых свойств текста. Читаемость — наиболее изученное из таких свойств. Данный репозиторий количественно описывает разрыв между классическими сказками, написанными человеком, и их пересказами от 19 LLM, по 6 устоявшимся индексам читаемости, с применением классических статистических инструментов.

## Исследовательский вопрос

*Воспроизводят ли LLM читаемость классических детских сказок, и если нет — по каким именно измерениям они расходятся?*

## Данные

- **overalldata.xlsx** — сырые значения метрик для каждой пары (модель × сказка), включая оригинальные тексты
- **combined_model_ranking_all.xlsx** — агрегированный рейтинг по всем метрикам

**Корпус:** 16 классических английских сказок из Project Gutenberg (*Henny Penny*, *Jack Hannaford*, *The Rose Tree*, *The Three Sillies* и ещё 12 текстов).

**Объём выборки:** каждая из 19 LLM пересказала все 16 сказок → 304 сгенерированных текста + 16 оригиналов.

## Методы

| Шаг | Инструмент | Назначение |
|---|---|---|
| Расчёт метрик | `textstat` | 6 индексов читаемости на каждый текст |
| Проверка распределения | `scipy`, `matplotlib` | Q-Q-графики, диагностика нормальности |
| Сравнение групп | `statsmodels` | однофакторный ANOVA на каждую метрику |
| Попарное сравнение | `statsmodels` (Tukey HSD) | какие модели различаются значимо, по какой метрике |
| Агрегированный рейтинг | `pandas` | синтез per-metric результатов в общий |

**Анализируемые индексы читаемости:**

- Automated Readability Index (ARI)
- Coleman–Liau Index
- Flesch–Kincaid Grade Level
- Flesch Reading Ease
- Gunning Fog
- SMOG Index

## Структура репозитория

```
15_Fairy_tales_Project_Gutenberg_Text_Readability Levels Across Models and Original Text.ipynb
    обзор значений читаемости по всем моделям и оригиналам

ANOVA_Automated_Readability_Index.ipynb
ANOVA_Coleman_Liau_Index.ipynb
ANOVA_Flesch_Kincaid_Grade_Level_.ipynb
ANOVA_Flesch_Reading_Ease.ipynb
ANOVA_Gunning_Fog.ipynb
ANOVA_SMOG_Index.ipynb
    однофакторный ANOVA для каждой из метрик

Tukey's_HSD_tests.ipynb
    post-hoc попарные сравнения между моделями

Q_Q_Plot_readability_metrics.ipynb
    диагностика нормальности распределений

Grouped_metrics_based_results.ipynb
    агрегированный анализ по группам метрик

Combined_model_ranking_results.ipynb
    синтез: какие модели лучше / хуже по каждой метрике

overalldata.xlsx
combined_model_ranking_all.xlsx
    наборы данных, используемые в ноутбуках
```

## Ключевые выводы

Ни одна модель не воспроизводит читаемость оригиналов полностью; отдельные LLM превосходят других в конкретных измерениях.
Тексты, написанные людьми, образуют отдельный кластер по результатам ANOVA почти по всем индексам — то есть разрыв между LLM и человеческими нарративами остаётся значимым.
Подробные рейтинги по каждой метрике и результаты значимости — в `Combined_model_ranking_results.ipynb`.

## Как воспроизвести

```bash
pip install textstat statsmodels scipy pandas matplotlib plotly openpyxl jupyter
jupyter notebook
```

Ноутбуки подгружают `.xlsx`-файлы напрямую, никаких внешних загрузок не требуется.

## Ограничения

- Корпус из 16 сказок мал; bootstrap / ресэмплинг мог бы усилить статистику
- Индекс Dale–Chall опирается на фиксированный список «сложных» слов, который не отражает специфику детского словаря
- Метрики читаемости измеряют поверхностную сложность, а не семантическую связность или качество нарратива

## Связанные репозитории

- [Story-generation-process](https://github.com/MarinaBaklanova17/Story-generation-process) — пайплайн, сгенерировавший LLM-тексты для этого анализа
- Магистерская работа: *The comparative analysis of human-written and large language model generated children's tales: Readability and accuracy assessments of LLM-generated texts*, Whitireia New Zealand (2025)

## Лицензия

Лицензия не указана; для коммерческого использования, пожалуйста, откройте issue.

---

*Язык:* [English](README.md) · **Русский**
