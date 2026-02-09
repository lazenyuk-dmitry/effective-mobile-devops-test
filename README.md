# Effective Mobile — DevOps Test Task

Тестовое задание на позицию Junior DevOps Engineer.

## Описание

Проект представляет собой простое веб-приложение, состоящее из двух сервисов:
- **Backend** — минимальный HTTP-сервер на Python
- **Nginx** — reverse proxy, проксирующий запросы к backend

Все сервисы запускаются в Docker-контейнерах и взаимодействуют между собой через docker-сеть.

---

## Реализация и особенности

- Используются официальные Docker-образы
- Backend запускается от непривилегированного пользователя
- Реализован HEALTHCHECK для backend-сервиса
- В nginx используется проксирование по имени сервиса (backend)
- Внешний доступ открыт только к nginx

---

## Используемые технологии

- Docker
- Docker Compose
- Nginx
- Python (http.server)
- Linux

---

## Архитектура

```
Client
| HTTP :80
v
Nginx (container)
| HTTP :8080 (docker network)
v
Backend (container)
```

- Backend **не доступен напрямую с хоста**
- Снаружи опубликован только порт `80` nginx

---

## Структура проекта

```
.
├── backend/
│ ├── Dockerfile
│ └── app.py
├── nginx/
│ └── nginx.conf
├── docker-compose.yml
├── README.md
└── .gitignore
```

---

## Требования

- Docker
- Docker Compose

---

## Запуск проекта

Из корня проекта выполнить:

```bash
docker compose up --build
```

---

## Проверка работоспособности

```bash
curl http://localhost
```

Ожидаемый ответ:

`Hello from Effective Mobile!`

---

## Проверка изоляции backend

Backend не должен быть доступен напрямую с хоста:

```bash
curl http://localhost:8080
```

Ожидаемо — соединение не устанавливается.

---

## Проверка пользователя контейнера

Backend-сервис запускается от непривилегированного пользователя.

Для проверки можно выполнить:

```bash
docker exec -it backend id
```

Ожидаемый результат:

`uid=1000(appuser) gid=1000(appuser) groups=1000(appuser)`
