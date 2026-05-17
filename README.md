# Демонстрационное приложение для курса Docker-контейнеризация и хранение данных

## 1. Контейнеризация фронтенда и бэкенда

Бекенд и фронтенд собраны в двух вариантах. Первый вариант, представленный в `Dockerfile` и `Dockerfile.dhi` с помощью hardened образов от компании Docker [1].

### Команды для сборки:

**Базовые образы:**
- `docker build -f backend/Dockerfile -t docker-project-backend:{version} .`
- `docker build -f frontend/Dockerfile --build-arg VUE_APP_API_URL=http://localhost:8081 -t docker-project-frontend:{version} .`

**DHI образы:**
- `docker build -f backend/Dockerfile.dhi -t docker-project-backend:{version} .`
- `docker build -f frontend/Dockerfile.dhi -t docker-project-frontend:{version} .`

### Команды для запуска:
- `docker run -d --name momo-backend -p 8081:8081 docker-project-backend`
- `docker run -d --name momo-frontend -p 80:80 docker-project-frontend`

## 2. Оптимизация размера образов

## 3. Конфигурируемость контейнеров

## 4. Инфраструктура и устойчивость приложения

## 5. Масштабируемость и балансировка нагрузки

## 6. Основы безопасности контейнеров

## 7. Управление секретами

## 8. Безопасность образов

***

**Список литературы:**
1. https://www.docker.com/products/hardened-images/
