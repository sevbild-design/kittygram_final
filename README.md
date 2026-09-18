# Kittygram

[![Main Kittygram workflow](https://github.com/sevbild-design/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/sevbild-design/kittygram_final/actions/workflows/main.yml)

Kittygram — веб-приложение для публикации информации о домашних питомцах.

Пользователи могут регистрироваться, добавлять своих котов, загружать фотографии,
указывать цвет, год рождения и достижения питомца, а также редактировать и удалять
созданные записи.

Проект состоит из Django REST API и React-фронтенда. Приложение работает
в Docker-контейнерах, а тестирование, сборка образов и production-деплой
автоматизированы с помощью GitHub Actions.

## Возможности проекта

- регистрация и авторизация пользователей;
- токен-аутентификация;
- добавление питомцев;
- загрузка фотографий;
- редактирование и удаление питомцев;
- добавление достижений;
- хранение данных в PostgreSQL;
- раздача статических и медиафайлов через Nginx;
- запуск приложения в Docker-контейнерах;
- автоматическое тестирование и деплой через GitHub Actions;
- отправка уведомления в Telegram после успешного деплоя.

## Технологии

### Backend

- Python 3.12
- Django
- Django REST Framework
- Djoser
- PostgreSQL
- Gunicorn

### Frontend

- React
- JavaScript
- Node.js

### Infrastructure

- Docker
- Docker Compose
- Nginx
- GitHub Actions
- Docker Hub

## Архитектура

Production-окружение состоит из четырёх основных сервисов:

- `db` — база данных PostgreSQL;
- `backend` — Django-приложение, запущенное через Gunicorn;
- `frontend` — React-приложение;
- `gateway` — Nginx, который раздаёт фронтенд, статические и медиафайлы
  и проксирует запросы к backend.

На сервере дополнительно используется внешний Nginx. Он принимает HTTPS-запросы
на домен проекта и перенаправляет их на контейнер `gateway`.

## Переменные окружения

Конфиденциальные настройки проекта хранятся в файле `.env`.

Пример конфигурации находится в `.env.example`.

```env
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=your_password
DB_HOST=db
DB_PORT=5432

SECRET_KEY=your_secret_key
DEBUG=False
ALLOWED_HOSTS=your_domain
USE_SQLITE=False

DOCKER_USERNAME=your_dockerhub_username
```

| Переменная | Назначение |
| --- | --- |
| `POSTGRES_DB` | имя базы данных PostgreSQL |
| `POSTGRES_USER` | пользователь PostgreSQL |
| `POSTGRES_PASSWORD` | пароль пользователя PostgreSQL |
| `DB_HOST` | адрес сервера базы данных |
| `DB_PORT` | порт PostgreSQL |
| `SECRET_KEY` | секретный ключ Django |
| `DEBUG` | режим отладки Django |
| `ALLOWED_HOSTS` | разрешённые хосты Django |
| `USE_SQLITE` | выбор SQLite вместо PostgreSQL |
| `DOCKER_USERNAME` | имя пользователя на DockerHub |

Файл `.env` содержит конфиденциальные данные и не должен добавляться в Git.

## Production-деплой

Production-деплой выполняется автоматически с помощью GitHub Actions.

Workflow расположен в:

```text
.github/workflows/main.yml
```

Перед первым деплоем сервер должен быть подготовлен:

- установлен Docker;
- установлен и настроен внешний Nginx;
- настроен HTTPS;
- создана директория проекта;
- создан production-файл `.env`;
- в GitHub Actions добавлены необходимые Secrets.

После отправки изменений в ветку `main` GitHub Actions автоматически:

1. проверяет backend с помощью Flake8;
2. запускает тесты backend;
3. запускает тесты frontend;
4. собирает Docker-образы;
5. публикует образы на Docker Hub;
6. подключается к серверу по SSH;
7. загружает актуальные Docker-образы;
8. перезапускает production-контейнеры;
9. ожидает готовности PostgreSQL;
10. выполняет миграции Django;
11. собирает и копирует статические файлы;
12. отправляет уведомление в Telegram после успешного деплоя.

Имена Docker-образов:

```text
<dockerhub_username>/kittygram_backend
<dockerhub_username>/kittygram_frontend
<dockerhub_username>/kittygram_gateway
```

## Развёрнутый проект

Kittygram:

https://sprint.bounceme.net

## Автор

Милютиков Павел ([GitHub] https://github.com/sevbild-design)
