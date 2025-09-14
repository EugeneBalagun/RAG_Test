# RAG Pipeline Test

## Описание
Проект реализует RAG (Retrieval-Augmented Generation) пайплайн для ответа на вопросы на русском языке, используя Qdrant для векторного поиска (порт 6334) и модель facebook/mbart-large-50-many-to-many-mmt.  
Чанки статей хранятся в artifacts/rag_article.jsonl, эмбеддинги — в artifacts/embeddings.pkl, результаты запросов — в artifacts/rag_result.json.  
Логи запросов сохраняются в logs/query_log.json, журнал версий — в artifacts/version_log.json.  
Эксперименты логируются в MLflow (порт 5001).

## Установка
1. Установите Docker Desktop.
2. Клонируйте репозиторий:
  
   git clone <your-repo-url>
   cd project
   
4. Установите зависимости:
pip install -r requirements.txt

## Требуемые библиотеки:

transformers

sentencepiece

qdrant-client==1.12.0

langchain

sentence-transformers

mlflow==2.9.2

notebook



## Использование

Запустите сервисы:
docker-compose up -d

Запустите Jupyter Notebook:
jupyter notebook

Откройте rag_pipeline.ipynb и выполните блоки.

Блок LLM: инициализация модели
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

MODEL_NAME = "facebook/mbart-large-50-many-to-many-mmt"
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME, use_fast=False)
model = AutoModelForSeq2SeqLM.from_pretrained(MODEL_NAME)

Блоки 1–3
Загрузка статьи
Очистка текста
Создание чанков

Блок 4
Загрузка чанков и эмбеддингов в Qdrant

Блок 5
Обработка запроса
Генерация ответа
Логирование в MLflow

Проверка работы
Qdrant: http://localhost:6333
MLflow: http://localhost:5001

Выполните rag_pipeline.ipynb и проверьте:
вывод в ноутбуке
логи в logs/query_log.json
журнал версий в artifacts/version_log.json

MLflow UI с экспериментом (query, num_chunks, rag_result.json)


Пример запроса
query = "Что автор имеет в виду под 'иллюзией качества'?"

Ожидаемый вывод:

Вопрос: Что автор имеет в виду под 'иллюзией качества'?
Ответ: Внутри — ядро: логика, смысл, результат. Иллюзия Качества — это когда вы покупаете шелуху, перепутав ее с сердцевиной. Вы смотрите на сайт, как на витрину — и верите, что это бизнес-инструмент. А на самом деле — просто декор.

Релевантные чанки:
Чанк 1: рассекаем “Качество” Иллюзия Качества — это когда вы платите за лоск...
Чанк 2: анонимных перфекционистов Кстати, о “памятниках”...
Чанк 3: Поверхностное качество (Витрина) Что это? Всё, что можно оценить за 5 секунд...


Обновление статьи
Замените article.txt (или article.html) новой статьей.
Выполните блоки 1–4.
Запустите блок 5 с новым запросом.
Проверьте логи в MLflow: http://localhost:5001
