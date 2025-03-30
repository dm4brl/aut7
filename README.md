# aut7
# Система управления освещением

## Описание
Этот проект представляет собой распределенную систему управления освещением, основанную на стеке **PostgreSQL + Redis + EMQX + Kafka + Golang + Temporal**.

### **Основные функции**
- Управление умными свитчами через MQTT (EMQX)
- Хранение конфигураций и состояний в **PostgreSQL** с использованием **TimescaleDB**
- Кеширование данных и состояний устройств в **Redis**
- Очереди событий и потоковая обработка через **Kafka**
- Выполнение сложных задач и сценариев через **Temporal**
- Мониторинг и логирование через **Prometheus + Grafana**

## **Технологический стек**
- **PostgreSQL** – реляционная база данных (TimescaleDB для временных рядов)
- **Redis** – кеш и временное хранилище состояний
- **EMQX** – MQTT-брокер для связи с IoT-устройствами
- **Kafka** – очередь сообщений и обработка событий
- **Golang** – микросервисы и бизнес-логика
- **Temporal** – управление задачами и процессами
- **Kubernetes** – оркестрация сервисов
- **Prometheus + Grafana** – мониторинг и логирование

## **Структура проекта**
```
├── cmd/              # Основные исполняемые файлы
├── internal/         # Логика приложения
│   ├── services/     # Микросервисы
│   ├── database/     # Работа с PostgreSQL и Redis
│   ├── mqtt/         # Интеграция с EMQX
│   ├── queue/        # Обработка событий Kafka
├── configs/          # Конфигурационные файлы
├── deployments/      # Манифесты Kubernetes
├── scripts/          # Скрипты развертывания
├── README.md         # Документация проекта
```

## **Схема базы данных**
```sql
CREATE TABLE devices (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    location TEXT,
    status BOOLEAN DEFAULT FALSE,
    last_updated TIMESTAMP DEFAULT now()
);

CREATE TABLE schedules (
    id SERIAL PRIMARY KEY,
    device_id INT REFERENCES devices(id),
    event_time TIMESTAMP NOT NULL,
    action TEXT CHECK (action IN ('ON', 'OFF')),
    created_at TIMESTAMP DEFAULT now()
);

CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    device_id INT REFERENCES devices(id),
    event_time TIMESTAMP DEFAULT now(),
    action TEXT CHECK (action IN ('ON', 'OFF')),
    status TEXT CHECK (status IN ('SUCCESS', 'FAILED'))
);

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```

## **Установка и запуск**
```sh
git clone https://github.com/yourrepo/lighting-control.git
cd lighting-control
docker-compose up -d  # Запуск локальной среды
```

## **TODO**
- [ ] Реализация сервисов управления
- [ ] Интеграция с MQTT (EMQX)
- [ ] Подключение Kafka и обработка событий
- [ ] Настройка TimescaleDB
- [ ] Мониторинг через Prometheus и Grafana

---
С Б-гом!



---
С Б-гом!


