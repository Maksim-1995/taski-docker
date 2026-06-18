# Taski

Проект **Taski** — это простое и удобное приложение для управления задачами (todo-лист). Пользователи могут создавать, редактировать, просматривать и удалять задачи, отмечать их выполнение. Проект предназначен для отработки навыков контейнеризации Django-приложений с помощью Docker и настройки CI/CD.

## Использованные технологии

| Технология | Назначение | Документация |
|---|---|---|
| **Django 5.1.1** | Backend-фреймворк | https://docs.djangoproject.com/ |
| **Django REST Framework 3.15.2** | Создание REST API | https://www.django-rest-framework.org/ |
| **django-cors-headers 4.6.0** | Обработка CORS-запросов | https://github.com/adamchainz/django-cors-headers |
| **PostgreSQL** | База данных | https://www.postgresql.org/docs/ |
| **React** | Frontend-фреймворк | https://react.dev/ |
| **Nginx** | Веб-сервер и reverse-proxy | https://nginx.org/ |
| **Docker** | Контейнеризация приложения | https://docs.docker.com/ |
| **Docker Compose** | Оркестрация контейнеров | https://docs.docker.com/compose/ |
| **Gunicorn** | WSGI-сервер для Django | https://gunicorn.org/ |
| **GitHub Actions** | CI/CD (непрерывная интеграция и деплой) | https://docs.github.com/actions |
| **pytest** | Тестирование | https://docs.pytest.org/ |
| **flake8** | Линтер Python-кода | https://flake8.pycqa.org/ |

## Установка и запуск проекта

### Требования

- Установленные [Docker](https://docs.docker.com/get-docker/) и [Docker Compose](https://docs.docker.com/compose/install/)
- Клонированный репозиторий проекта

### Локальный запуск с помощью Docker

1. Клонируйте репозиторий:

```bash
git clone git@github.com:Maksim-1995/taski-docker.git
cd taski-docker
```

2. Создайте файл `.env` в корне проекта (пример в `.env.example`):

```
SECRET_KEY=django-insecure-ваш-секретный-ключ
DEBUG=True
POSTGRES_DB=django
POSTGRES_USER=django
POSTGRES_PASSWORD=password
DB_HOST=db
DB_PORT=5432
```

3. Запустите контейнеры:

```bash
docker compose up -d
```

4. Выполните миграции и сбор статики:

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic
docker compose exec backend cp -r /app/collected_static /app/static_backend
```

5. Проект будет доступен по адресу: [http://localhost:8000](http://localhost:8000), https://taski-maksim.duckdns.org

### Запуск тестов

```bash
# Создайте виртуальное окружение
python3 -m venv venv
source venv/bin/activate

# Установите зависимости
pip install -r backend/requirements.txt

# Запустите тесты
pytest
```

### Развёртывание на сервере

1. На сервере должны быть установлены Docker и Docker Compose.
2. Скопируйте файлы `docker-compose.production.yml` и `.env` на сервер.
3. Запустите:

```bash
docker compose -f docker-compose.production.yml up -d
```

4. Настройте CI/CD через GitHub Actions (см. файл `.github/workflows/main.yml`).

## Примеры запросов к API

### Получение списка задач

```bash
GET /api/tasks/
```

**Ответ:** `200 OK`

```json
[
  {
    "id": 1,
    "title": "Купить продукты",
    "description": "Молоко, хлеб, яйца",
    "completed": false
  },
  {
    "id": 2,
    "title": "Сделать зарядку",
    "description": "Утренняя пробежка 30 минут",
    "completed": true
  }
]
```

### Создание новой задачи

```bash
POST /api/tasks/
Content-Type: application/json

{
  "title": "Написать отчёт",
  "description": "Подготовить еженедельный отчёт",
  "completed": false
}
```

**Ответ:** `201 Created`

```json
{
  "id": 3,
  "title": "Написать отчёт",
  "description": "Подготовить еженедельный отчёт",
  "completed": false
}
```

### Получение задачи по ID

```bash
GET /api/tasks/1/
```

**Ответ:** `200 OK`

```json
{
  "id": 1,
  "title": "Купить продукты",
  "description": "Молоко, хлеб, яйца",
  "completed": false
}
```

### Обновление задачи

```bash
PATCH /api/tasks/1/
Content-Type: application/json

{
  "completed": true
}
```

**Ответ:** `200 OK`

```json
{
  "id": 1,
  "title": "Купить продукты",
  "description": "Молоко, хлеб, яйца",
  "completed": true
}
```

### Удаление задачи

```bash
DELETE /api/tasks/3/
```

**Ответ:** `200 OK`

```json
{
  "id": 3,
  "title": "Написать отчёт",
  "description": "Подготовить еженедельный отчёт",
  "completed": false
}
```


## Автор

**Максим** — [GitHub](https://github.com/Maksim-1995)