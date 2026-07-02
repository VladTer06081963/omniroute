# OmniRoute Data Storage

## Что такое OmniRoute?

OmniRoute — это готовый Docker сервис для управления маршрутизацией запросов к различным провайдерам. Исходный код находится в Docker образе `diegosouzapw/omniroute:latest`, а не в этом репо.

## Структура проекта

```
omniroute/
├── docker-compose.yaml       # Конфигурация для запуска Docker сервиса
├── .env                      # Переменные окружения (не отправляются в git)
├── .env.example              # Пример .env файла
├── README.md                 # Документация проекта
├── aider_runner.py           # Утилита
├── tg-bot/                   # Отдельный проект Telegram бота
└── summary/                  # Документация и описание
```

## Запуск OmniRoute

```bash
docker-compose up -d
```

Сервис будет доступен на `http://localhost:20128`

## Хранилище данных: omniroute-data

### Местоположение

```
/var/lib/docker/volumes/omniroute_omniroute-data/_data/
```

**Примечание:** требуется `sudo` для доступа к этой папке.

### Структура omniroute-data

```
omniroute-data/
├── call_logs/              # Логи всех запросов через API
├── codex-profiles/         # Профили и конфигурации
├── db_backups/             # Резервные копии базы данных
├── oauth/                  # Данные OAuth
├── server.env              # Конфигурация сервера
├── storage.sqlite          # Основная SQLite база данных
├── storage.sqlite-shm      # Служебный файл SQLite (shared memory)
└── storage.sqlite-wal      # Служебный файл SQLite (write-ahead log)
```

### Что хранится?

- **storage.sqlite** — главная БД, содержит:
  - Конфигурация провайдеров
  - Данные пользователей
  - Настройки сервиса

- **call_logs/** — история всех API запросов с:
  - Параметрами запроса
  - Ответами провайдеров
  - Временными метками

- **db_backups/** — автоматические резервные копии

- **oauth/** — сохраненные данные OAuth интеграций

## Переменные окружения

Файл `.env` должен содержать:

```env
PORT=20128
NEXT_PUBLIC_BASE_URL=http://localhost:20128
INITIAL_PASSWORD=<ваш пароль>
ENABLE_REQUEST_LOGS=true
GOOGLE_CLIENT_ID=<ваш ID>
GOOGLE_CLIENT_SECRET=<ваш secret>
```

## API примеры

### cURL
```bash
curl -X POST http://localhost:20128/api/route \
  -H "Content-Type: application/json" \
  -d '{"provider": "example", "data": {"key": "value"}}'
```

### Python
```python
import requests

response = requests.post(
    "http://localhost:20128/api/route",
    json={"provider": "example", "data": {"key": "value"}}
)
print(response.json())
```

## Важно

- Данные в volume сохраняются между перезапусками контейнера
- При удалении volume все данные будут потеряны
- Для бэкапа используйте файлы из `db_backups/` или сделайте dump базы данных
