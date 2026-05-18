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

### Команды для запуска:

**Создание сети:**
- `docker network create momo-network`

**Запуск контейнеров:**
- `docker run -d --name momo-backend --network momo-network -p 8081:8081 docker-project-backend:{version}`
- `docker run -d --name momo-frontend --network momo-network -p 80:8080 docker-project-frontend:{version}`

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
