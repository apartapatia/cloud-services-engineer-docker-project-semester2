# Демонстрационное приложение для курса Docker-контейнеризация и хранение данных

## 1. Контейнеризация фронтенда и бэкенда

Бекенд и фронтенд собраны в двух вариантах. Первый вариант, представленный в `Dockerfile` и `Dockerfile.dhi` с помощью hardened образов от компании Docker [1].

### Команды для сборки:

**Базовые образы:**

- `docker build -f backend/Dockerfile -t docker-project-backend:{version} backend/`
- `docker build -f frontend/Dockerfile -t docker-project-frontend:{version} frontend/`

**DHI образы:**

- `docker build -f backend/Dockerfile.dhi -t docker-project-backend:{version} backend/`
- `docker build -f frontend/Dockerfile.dhi -t docker-project-frontend:{version} frontend/`

### Команды для запуска через cli:

**Создание сети:**

- `docker network create momo-network`

**Запуск контейнеров:**

- `docker run -d --name momo-backend --network momo-network -p 8081:8081 docker-project-backend:{version}`
- `docker run -d --name momo-frontend --network momo-network -p 80:8080 docker-project-frontend:{version}`

### Команды для запуска через Docker Compose:

- Сборка и запуск: `docker compose up -d --build`
- Остановка: `docker compose down`

## 2. Оптимизация размера образов

Для оптимизации образов были созданы dockerignore файлы, которые не позволяют попасть лишней информации в готовый образ. Также каждый докерфайл собираться с помощью multi stage build, с оптимизацией кеша для фронта и последовательным запуском RUN команд в одном слое.

**Итоговые размеры образов:**

```
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
docker-project-frontend   latest    ace76e423b17   53 seconds ago   50MB
docker-project-backend    latest    ae66f03c3b84   2 minutes ago    20.3MB
```

## 3. Конфигурируемость контейнеров

Вся конфигурация хранится в .env файле, что позволяет задать порты для фронта и бекенда, также URL взаимодействия двух сервисов и версию собранных имеджей.

## 4. Инфраструктура и устойчивость приложения

Приложение разворачивается с одного компоуз файла, с возможностью балансировки. Бекенд поднимается с ограничениям по ресурсам, для хапрокси и фронтенда это излишне.

## 5. Масштабируемость и балансировка нагрузки

Масштабируемость выполненна за счет стоящего перед сервисами хапрокси, при скейле кол-ва инстансов хапрокси обращается в днс докера и собирает инстансы для балансировки. Все запросы сначала попадают в хапрокси, потом уходят на балансированные компоненты.

`docker compose up -d --scale momo-backend=7 --scale momo-frontend=3`

Для теста поднял 7 реплик бекенда и 3 фронта, с помощью хапрокси и curl запросов были сняты метрики балансировки.

| Пул                  | Имя сервера в HAProxy | Обработано запросов |
| :------------------- | :-------------------- | :-----------------: |
| **frontend_servers** | frontend 1            |         69          |
|                      | frontend 2            |         69          |
|                      | frontend 3            |         68          |
| **backend_servers**  | backend 1             |         49          |
|                      | backend 2             |         48          |
|                      | backend 3             |         48          |
|                      | backend 4             |         15          |
|                      | backend 5             |         15          |
|                      | backend 6             |         14          |
|                      | backend 7             |         14          |

## 6. Основы безопасности контейнеров

## 7. Управление секретами

Секреты передаются с помощью .env файлов.

## 8. Безопасность образов

---

**Список литературы:**

1. https://www.docker.com/products/hardened-images/
