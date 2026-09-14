# Kittygram

Социальная сеть для публикации фотографий котиков🧶 /ᐠ. .ᐟ\ฅ Учебный проект, выполненный
в рамках курса «Python-разработчик» Яндекс Практикума и используемый как
портфолио-проект.

## О проекте

Kittygram — это веб-приложение, где пользователи могут:
- регистрироваться и авторизовываться;
- добавлять, редактировать и удалять карточки своих котиков;
- загружать фотографии питомцев;
- указывать имя, год рождения и цвет котика;
- просматривать карточки других пользователей;
- отмечать достижения питомцев.

Проект полностью контейнеризирован и разворачивается автоматически через
CI/CD. Демонстрирует навыки работы с Docker, Docker Compose, Nginx, PostgreSQL.

## Стек технологий

**Backend:**
- Python 3.x
- Django
- Django REST Framework
- PostgreSQL
- Gunicorn

**Frontend:**
- React
- Node.js

**Инфраструктура:**
- Docker / Docker Compose
- Nginx (gateway)
- GitHub Actions (CI/CD)
- Docker Hub (хранение образов)


## Функциональность

- Регистрация и аутентификация пользователей
- CRUD для карточек котиков
- Загрузка фотографий питомцев
- REST API на Django REST Framework
- Раздача статики и медиа через Nginx
- Полная контейнеризация проекта
- Автоматическое тестирование при пуше
- Автоматический деплой на сервер
- Уведомления в Telegram о статусе деплоя

## CI/CD

1. **Проверка кода** — flake8 на соответствие PEP8
2. **Тестирование** — запуск тестов backend и frontend
3. **Сборка образов** — сборка `kittygram_backend`, `kittygram_frontend`,
   `kittygram_gateway`
4. **Пуш на Docker Hub** — отправка образов в реестр
5. **Деплой на сервер** — обновление образов и перезапуск контейнеров
   через Docker Compose
6. **Миграции и сборка статики** — выполнение внутри контейнера backend
7. **Уведомление в Telegram** — сообщение об успешном деплое

## Установка и запуск

### 1. Клонировать репозиторий

```bash
git clone https://github.com/GoloveshkinaMaria-Dev/ya_kittygram.git
cd ya_kittygram
```

### 2. Создать `.env` в корне проекта

Пример — в файле `.env.example`. Скопируйте и заполните:

```bash
cp .env.example .env
```

Содержимое `.env`:
```env
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=your_strong_password
DB_HOST=db
DB_PORT=5432
SECRET_KEY=your_django_secret_key
DEBUG=False
ALLOWED_HOSTS=localhost,127.0.0.1,your_domain
```

### 3. Запустить контейнеры

**Локально (для разработки):**
```bash
docker compose up -d --build
```

**На сервере (production):**
```bash
docker compose -f docker-compose.production.yml up -d
```

### 4. Выполнить миграции и собрать статику

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic --no-input
```

### 5. Создать суперпользователя

```bash
docker compose exec backend python manage.py createsuperuser
```

Проект будет доступен по адресу: http://localhost:9000/
Админ-панель: http://localhost:9000/admin/


## Деплой на сервер


```bash
# На сервере
cd ~/kittygram

# Обновить образы
docker compose -f docker-compose.production.yml pull

# Перезапустить контейнеры
docker compose -f docker-compose.production.yml up -d

# Миграции
docker compose -f docker-compose.production.yml exec backend python manage.py migrate

# Статика
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic --no-input
```

## GitHub Actions

В настройках репозитория (**Settings → Secrets and variables → Actions**)
должны быть заданы:

| Секрет | Описание |
|---|---|
| `DOCKER_USERNAME` | Логин на Docker Hub |
| `DOCKER_PASSWORD` | Пароль/токен Docker Hub |
| `HOST` | IP удалённого сервера |
| `USER` | Имя пользователя на сервере |
| `SSH_KEY` | Приватный SSH-ключ |
| `SSH_PASSPHRASE` | Пароль SSH-ключа |
| `TELEGRAM_TO` | ID чата в Telegram |
| `TELEGRAM_TOKEN` | Токен Telegram-бота |

## *ฅ^•ﻌ•^ฅ*
