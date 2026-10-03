# Навигатор по смыслу — сравнение embedding-моделей для семантического поиска

Учебная практика 1 курс, МТУСИ, кафедра МКИТ, БВТ25. Кейс Ростелекома (чемпионат ТОП-ИТ/ТОП-ИИ).

## Задача
Построить retrieval-систему: по текстовому запросу находить релевантные фрагменты кода из корпуса 200 функций (100 Python + 100 Java). Оценка по 25 тестовым вопросам (15 RU + 10 EN) с метриками Precision@3 и MRR.

## Данные
- `data/code_corpus.json` — 200 функций (id, language, function_name, code, description, category)
- `data/eval_questions.json` — 25 вопросов (query, language, correct_chunk_id)
- `data/categories.json` — 5 категорий с цветами для визуализации

## Модели
| Модель | Размерность | Особенность |
|--------|-------------|-------------|
| paraphrase-multilingual-MiniLM-L12-v2 | 384 | Быстрая, мультиязычная |
| paraphrase-multilingual-mpnet-base-v2 | 768 | Качественная, мультиязычная |
| intfloat/multilingual-e5-small | 384 | Retrieval-семейство E5, query/passage префиксы |

Текст для кодирования: `description — function_name` (без исходного кода).

## Результаты

| Модель | Precision@3 | MRR | Время кодирования (сек) |
|--------|-------------|-----|------------------------|
| **e5-small** | **1.000** | **0.833** | 0.5 |
| mpnet-base | 0.920 | 0.627 | 0.9 |
| MiniLM-L12 | 0.800 | 0.580 | 1.1 |

### По языкам запроса
| Модель | RU (15) | EN (10) |
|--------|---------|---------|
| e5-small | 1.000 | 1.000 |
| mpnet-base | 0.933 | 0.900 |
| MiniLM-L12 | 0.733 | 0.900 |

Лучшая модель — **e5-small**: идеальный Precision@3 на всех категориях, 0 ошибок, стабильна на обоих языках.

## Запуск
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook solution.ipynb
# Cell → Run All
```

Артефакты после прогона в `results/`:
- `results_table.csv` — таблица метрик
- `bar_p3.png` — столбиковая диаграмма
- `tsne_best.png` — t-SNE проекция эмбеддингов e5-small
- `errors.txt` — ошибки (пусто, 0 шт.)
- `final_conclusion.txt` — черновик выводов (переписать своими словами перед сдачей)

## Требования
- Python 3.12+ (проверено на 3.14)
- Зависимости в `requirements.txt`
