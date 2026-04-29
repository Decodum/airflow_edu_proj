# ☁️ Data Engineering Pipeline with Apache Airflow

[![Python](https://img.shields.io/badge/python-3.9-blue.svg)](https://www.python.org/)
[![Airflow](https://img.shields.io/badge/airflow-2.3.3-green.svg)](https://airflow.apache.org/)
[![Postgres](https://img.shields.io/badge/postgres-13-blue.svg)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/docker-compose-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## 📖 О проекте

Этот учебный проект демонстрирует создание полноценного ETL-пайплайна на основе **Apache Airflow**. Цель — автоматизировать процесс извлечения данных о курсах криптовалют через **API**, их обработки и последующей загрузки в **PostgreSQL**. Проект полностью контейнеризирован с помощью **Docker Compose**, что позволяет легко развернуть и протестировать инфраструктуру, типичную для production-среды.

В рамках проекта настроен полный цикл работы Data Engineer:
*   🐳 **Оркестрация контейнеров**: Развёртывание Airflow, PostgreSQL и Redis.
*   ⏱️ **Планирование и мониторинг**: Создание DAG'а для периодического запуска задач.
*   💾 **Работа с базами данных**: Автоматическое создание таблиц и запись данных в PostgreSQL.
*   🌐 **Взаимодействие с API**: Извлечение данных в реальном времени из внешнего источника.

## 🧱 Архитектура решения

Проект состоит из нескольких сервисов, которые работают в изолированной среде Docker:

1.  **PostgreSQL**: Основная база данных для хранения метаданных Airflow и целевая база для результатов ETL.
2.  **Redis**: Используется в качестве брокера сообщений для CeleryExecutor, что позволяет распределять задачи между воркерами.
3.  **Airflow-Webserver**: Веб-интерфейс для управления DAG, мониторинга логов и отслеживания статусов задач.
4.  **Airflow-Scheduler**: Сердце Airflow, отвечающее за запуск запланированных задач.
5.  **Airflow-Worker**: Исполнитель задач, который обрабатывает инструкции из DAG.
6.  **Airflow-Triggerer**: Необходим для работы с "отложенными" (deferrable) операторами, оптимизируя использование ресурсов.


## 🧩 Структура проекта

```bash
.
├── .gitignore                 # Исключения для системы контроля версий
├── docker-compose.yml         # Конфигурация для развертывания всех сервисов
├── airflow/
│   ├── dags/                  # Директория с DAG файлами
│   │   └── say_good_morning.py  # 🔄 Основной DAG (ETL пайплайн)
│   ├── logs/                  # Логи выполнения задач (игнорируется git)
│   └── plugins/               # Пользовательские плагины (пустая)
├── data/
│   └── say_good_morning.py    # ⚙️ Резервная или тестовая версия скрипта
└── postgres/
    └── init_scripts/          # SQL скрипты для инициализации БД
        └── task1.sql          # Создание таблицы "test_table"
