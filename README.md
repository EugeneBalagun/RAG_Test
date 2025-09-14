RAG Pipeline Test
Описание
Проект реализует RAG (Retrieval-Augmented Generation) пайплайн для ответа на вопросы на русском языке, используя базу векторного поиска Qdrant (порт 6334) и модель facebook/mbart-large-50-many-to-many-mmt. Чанки статей хранятся в artifacts/rag_article.jsonl, эмбеддинги — в artifacts/embeddings.pkl, результаты запросов — в artifacts/rag_result.json. Логи запросов сохраняются в logs/query_log.json, журнал версий — в artifacts/version_log.json. Эксперименты логируются в MLflow (порт 5001).
Установка

Установите Docker Desktop.
Клонируйте репозиторий:git clone <your-repo-url>
cd project


Установите зависимости:pip install -r requirements.txt

Требуемые библиотеки:
transformers
sentencepiece
qdrant-client==1.12.0
langchain
sentence-transformers
mlflow==2.9.2
notebook


Запустите сервисы Qdrant и MLflow:docker-compose up -d



Использование

Запустите Jupyter Notebook:jupyter notebook


Откройте rag_pipeline.ipynb и выполните блоки:
Блок LLM: Инициализация модели:from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
MODEL_NAME = "facebook/mbart-large-50-many-to-many-mmt"
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME, use_fast=False)
model = AutoModelForSeq2SeqLM.from_pretrained(MODEL_NAME)


Блоки 1–3: Загрузка статьи, очистка, создание чанков.
Блок 4: Загрузка чанков и эмбеддингов в Qdrant.
Блок 5: Обработка запроса, генерация ответа, логирование в MLflow.


Проверьте сервисы:
Qdrant: http://localhost:6333
MLflow: http://localhost:5001



Структура файлов

artifacts/:
article.txt (или article.html): Исходная статья.
metadata.json: Метаданные статьи.
cleaned_article.txt: Очищенный текст статьи.
rag_article.jsonl: Чанки статей для RAG.
embeddings.pkl: Эмбеддинги чанков для Qdrant.
rag_result.json: Результат последнего запроса (вопрос, ответ, чанки).
version_log.json: Журнал версий файлов.
README.md: Инструкции.


logs/:
query_log.json: Логи запросов и ответов.


mlflow_data/:
mlflow.db: SQLite база для MLflow.


mlflow_artifacts/: Артефакты экспериментов MLflow.
rag_pipeline.ipynb: End-to-end пайплайн.
docker-compose.yml: Конфигурация Qdrant и MLflow.
requirements.txt: Зависимости проекта.

Пример запроса
В блоке 5 задайте запрос:
query = "Что автор имеет в виду под 'иллюзией качества'?"

Ожидаемый вывод:
Вопрос: Что автор имеет в виду под 'иллюзией качества'?
Ответ: Внутри — ядро: логика, смысл, результат. Иллюзия Качества — это когда вы покупаете шелуху, перепутав ее с сердцевиной. Вы смотрите на сайт, как на витрину — и верите, что это бизнес-инструмент. А на самом деле — просто декор.
Релевантные чанки:
Чанк 1: рассекаем “Качество” Иллюзия Качества — это когда вы платите за лоск...
Чанк 2: анонимных перфекционистов Кстати, о “памятниках”...
Чанк 3: Поверхностное качество (Витрина) Что это? Всё, что можно оценить за 5 секунд...

Проверка

Запустите сервисы:docker-compose up -d


Проверьте доступность:
Qdrant: http://localhost:6333
MLflow: http://localhost:5001


Выполните rag_pipeline.ipynb.
Проверьте:
Вывод notebook (см. пример выше).
Логи: logs/query_log.json.
Журнал версий: artifacts/version_log.json.
MLflow UI: http://localhost:5001 (run с параметрами query, num_chunks и артефактом rag_result.json).



Обновление

Замените article.txt (или article.html) на новую статью.
Выполните блоки 1–4 для обработки новой статьи.
Выполните блок 5 с новым запросом.
Проверьте логи в MLflow: http://localhost:5001.
