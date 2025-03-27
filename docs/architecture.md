arct_ai_project/
├── .gitignore               # Файл для Git, чтобы игнорировать ненужные файлы (данные, логи, env)
├── README.md                # Описание проекта, инструкции по установке и запуску
├── requirements.txt         # Список зависимостей Python (или pyproject.toml для Poetry/PDM)
├── LICENSE                  # Лицензия проекта
│
├── config/                  # Конфигурационные файлы (не секретные)
│   ├── settings.yaml        # Общие настройки приложения
│   └── model_params.yaml    # Параметры для обучения моделей
│
├── data/                    # Все данные, связанные с проектом
│   ├── raw/                 # Исходные, необработанные данные
│   │   ├── meteo/           # Данные от метеослужб или локальной станции
│   │   │   └── meteo_data_*.csv
│   │   ├── dc_ops/          # Данные о работе ЦОД (BMS/DCIM)
│   │   │   └── datacenter_ops_*.csv
│   │   ├── arct_sensors/    # Данные с датчиков ARCT
│   │   │   └── arct_sensor_data_*.csv
│   │   └── sky_images/      # Изображения неба с камер
│   │       └── timestamp_cam1.jpg
│   │
│   ├── processed/           # Обработанные, очищенные, объединенные данные
│   │   └── combined_dataset_validated.parquet # Пример (parquet часто эффективнее CSV)
│   │
│   └── training/            # Данные, подготовленные специально для обучения моделей
│       ├── tilt_prediction/ # Данные для модели предсказания угла
│       │   ├── train.csv
│       │   └── validation.csv
│       ├── sky_analysis/    # Размеченные изображения неба
│       │   ├── train/
│       │   └── val/
│       └── ...              # Подпапки для данных других моделей
│
├── docs/                    # Документация проекта
│   ├── architecture.md      # Диаграмма архитектуры (например, с Mermaid)
│   ├── setup.md             # Инструкции по настройке окружения
│   ├── data_sources.md      # Описание источников данных
│   └── model_cards/         # Документация по каждой модели (цель, данные, метрики)
│       └── tilt_prediction.md
│
├── models/                  # Сохраненные обученные модели (артефакты)
│   ├── tilt_prediction/
│   │   └── v1.0/
│   │       └── model.pkl    # Или .h5, .pt, .onnx и т.д.
│   ├── sky_analysis/
│   │   └── latest/
│   │       └── model.h5
│   ├── health_monitoring/
│   └── ...                  # Папки для других моделей
│
├── notebooks/               # Jupyter-ноутбуки для исследований и экспериментов
│   ├── 1_eda_meteo.ipynb    # Исследовательский анализ метеоданных
│   ├── 2_prototype_tilt_model.ipynb # Прототипирование модели предсказания угла
│   └── 3_results_visualization.ipynb # Визуализация результатов
│
├── src/                     # Основной исходный код приложения (пакет Python)
│   ├── __init__.py
│   │
│   ├── data_processing/     # Модули для загрузки, очистки, трансформации данных
│   │   ├── __init__.py
│   │   ├── load_data.py
│   │   └── preprocessing.py
│   │
│   ├── features/            # Создание признаков для моделей
│   │   └── __init__.py
│   │   └── build_features.py
│   │
│   ├── models/              # Код, связанный с моделями ИИ
│   │   ├── __init__.py
│   │   ├── prediction/      # Модели предсказания (угол, ТО, деградация)
│   │   │   ├── __init__.py
│   │   │   ├── tilt_predictor.py
│   │   │   └── maintenance_predictor.py
│   │   ├── monitoring/      # Модели мониторинга (аномалии)
│   │   │   └── anomaly_detector.py
│   │   └── vision/          # Модели компьютерного зрения (анализ неба)
│   │       └── sky_analyzer.py
│   │
│   ├── training/            # Скрипты и пайплайны для обучения моделей
│   │   ├── __init__.py
│   │   ├── train_tilt_model.py
│   │   └── train_sky_model.py
│   │
│   ├── orchestration/       # Логика управления и интеграции ("мозг" системы)
│   │   ├── __init__.py
│   │   └── control_loop.py
│   │
│   ├── interfaces/          # Код для взаимодействия с внешними системами
│   │   ├── __init__.py
│   │   ├── bms_interface.py
│   │   ├── arct_actuator_interface.py
│   │   └── weather_api_client.py
│   │
│   ├── api/                 # Если будет REST API для управления или мониторинга
│   │   ├── __init__.py
│   │   └── main.py          # (e.g., FastAPI, Flask)
│   │   └── endpoints/
│   │
│   └── utils/               # Вспомогательные функции, константы, логирование
│       ├── __init__.py
│       └── logging_config.py
│
├── deployment/              # Файлы для развертывания
│   ├── Dockerfile           # Dockerfile для сборки приложения
│   ├── docker-compose.yml   # (Опционально) Для запуска нескольких контейнеров
│   └── scripts/             # Скрипты для CI/CD или ручного развертывания
│
├── tests/                   # Автоматические тесты
│   ├── __init__.py
│   ├── test_data_processing.py
│   ├── test_orchestration.py
│   └── test_models/
│       └── test_tilt_predictor.py
│
└── scripts/                 # Вспомогательные скрипты (запуск задач, обработка данных)
    └── process_raw_data.sh