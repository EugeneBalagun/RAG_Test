# RAG Pipeline Test

## Описание
Проект реализует **RAG (Retrieval-Augmented Generation) пайплайн** для ответов на вопросы на русском языке.  

Основные компоненты:
- **Векторный поиск** — база Qdrant (порт 6334).  
- **Языковая модель** — `facebook/mbart-large-50-many-to-many-mmt`.  

Хранение данных:
- Чанки статей — `artifacts/rag_article.jsonl`.  
- Эмбеддинги — `artifacts/embeddings.pkl`.  
- Результаты запросов — `artifacts/rag_result.json`.  

Логирование:
- Логи запросов — `logs/query_log.json`.  
- Журнал версий — `artifacts/version_log.json`.  
- Эксперименты — в MLflow (порт 5001).  

## Установка

1. Установите **Docker Desktop**.  
2. Клонируйте репозиторий:  
   ```bash
   git clone <your-repo-url>
   cd project
Установите зависимости:
pip install -r requirements.txt


## Требуемые библиотеки

- transformers  
- sentencepiece  
- qdrant-client==1.12.0  
- langchain  
- sentence-transformers  
- mlflow==2.9.2  
- notebook  




## Использование

### 1. Запуск сервисов

```bash
docker-compose up -d
```

### 2. Запуск Jupyter Notebook

```bash
jupyter notebook
```

Откройте файл **rag\_pipeline.ipynb** и выполняйте блоки по порядку:

---

### Блок LLM: инициализация модели

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

MODEL_NAME = "facebook/mbart-large-50-many-to-many-mmt"
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME, use_fast=False)
model = AutoModelForSeq2SeqLM.from_pretrained(MODEL_NAME)
```

---

### Блоки 1–3

* Загрузка статьи
* Очистка текста
* Создание чанков

### Блок 4

* Загрузка чанков и эмбеддингов в Qdrant

### Блок 5

* Обработка запроса
* Генерация ответа
* Логирование в MLflow

---

### Проверка работы

* Qdrant: [http://localhost:6333](http://localhost:6333)
* MLflow: [http://localhost:5001](http://localhost:5001)

Выполните `rag_pipeline.ipynb` и проверьте:

* вывод в ноутбуке
* логи: `logs/query_log.json`
* журнал версий: `artifacts/version_log.json`
* MLflow UI с экспериментом (`query`, `num_chunks`, `rag_result.json`)

---

### Пример запроса

```python
query = "Что автор имеет в виду под 'иллюзией качества'?"
```

**Ожидаемый вывод:**

```
Вопрос: Что автор имеет в виду под 'иллюзией качества'?
Ответ: Внутри — ядро: логика, смысл, результат. Иллюзия Качества — это когда вы покупаете шелуху, перепутав ее с сердцевиной. Вы смотрите на сайт, как на витрину — и верите, что это бизнес-инструмент. А на самом деле — просто декор.

Релевантные чанки:
Чанк 1: рассекаем “Качество” Иллюзия Качества — это когда вы платите за лоск...
Чанк 2: анонимных перфекционистов Кстати, о “памятниках”…
Чанк 3: Поверхностное качество (Витрина) Что это? Всё, что можно оценить за 5 секунд...
```



## Обновление статьи

1. Замените файл **article.txt** (или **article.html**) новой статьёй.  
2. Выполните блоки **1–4** для обработки текста:  
   - загрузка статьи  
   - очистка текста  
   - создание чанков  
   - загрузка в Qdrant  
3. Запустите **блок 5** с новым запросом.  
4. Проверьте логи в **MLflow**: [http://localhost:5001](http://localhost:5001).

