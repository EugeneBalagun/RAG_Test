# RAG Pipline test
RAG Pipeline Project
Описание
Проект реализует RAG (Retrieval-Augmented Generation) пайплайн для ответа на вопросы на русском языке, используя базу векторного поиска Qdrant и модель facebook/mbart-large-50-many-to-many-mmt. Чанки статей хранятся в rag_article.jsonl, эмбеддинги — в embeddings.pkl, результаты запросов — в rag_result.json. Логи запросов сохраняются в logs/query_log.json, журнал версий — в artifacts/version_log.json. 

Установка
Установите зависимости:pip install transformers sentencepiece qdrant-client langchain sentence-transformers

Убедитесь, что Qdrant запущен на localhost.

Использование
Запустите блок инициализации LLM:from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
MODEL_NAME = "facebook/mbart-large-50-many-to-many-mmt"
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME, use_fast=False)
model = AutoModelForSeq2SeqLM.from_pretrained(MODEL_NAME)


Выполните блок 4 для загрузки чанков и эмбеддингов в Qdrant.
Выполните блок 5 для обработки запроса и генерации ответа.

Структура файлов

artifacts/rag_article.jsonl: Чанки статей.
artifacts/embeddings.pkl: Эмбеддинги чанков.
artifacts/rag_result.json: Результат последнего запроса.
artifacts/version_log.json: Журнал версий RAG-файлов.
logs/query_log.json: Лог запросов и ответов.

Пример запроса
query = "Что автор имеет в виду под 'иллюзией качества'?"

Ожидаемый вывод:
Вопрос: Что автор имеет в виду под 'иллюзией качества'?
Ответ: "Иллюзия качества" — это акцент на внешнем виде цифрового продукта без учёта функциональности, что приводит к пустым тратам.
Релевантные чанки:
Чанк 1: рассекаем “Качество” Иллюзия Качества — это когда вы платите за лоск...
Чанк 2: Поверхностное качество (Витрина) Что это? Всё, что можно оценить за 5 секунд...
Чанк 3: анонимных перфекционистов Кстати, о “памятниках”...
