# Task API: Spring Boot + PostgreSQL + Docker Compose

CRUD API для сущности **Task** (сервис задач). Данные хранятся в PostgreSQL (`postgres:16`),
API и БД запускаются одной командой через Docker Compose.

## Запуск
```bash
docker compose up -d --build
docker compose ps
docker compose logs api
```

## CRUD-проверка (curl)
```bash
curl -X POST http://localhost:8080/tasks -H "Content-Type: application/json" -d '{"title":"Task 1","done":false}'
curl http://localhost:8080/tasks
curl -X PUT http://localhost:8080/tasks/1 -H "Content-Type: application/json" -d '{"title":"Task 1 updated","done":true}'
curl -X DELETE http://localhost:8080/tasks/1
```

## Проверка сохранения данных
```bash
curl -X POST http://localhost:8080/tasks -H "Content-Type: application/json" -d '{"title":"Persist me","done":false}'
docker compose down        # без -v, volume db_data остаётся
docker compose up -d
curl http://localhost:8080/tasks   # задача на месте
```

## Конфигурация
- Сервисы: `api` (Spring Boot, порт 8080) и `db` (postgres:16)
- API подключается к БД по имени сервиса: `jdbc:postgresql://db:5432/app` (передаётся через environment)
- Сеть: `app_net`; порядок запуска: `depends_on` + healthcheck (`pg_isready`)
- Volume: `db_data` -> `/var/lib/postgresql/data`

## Скриншоты
Лежат в папке `screenshots/`.
