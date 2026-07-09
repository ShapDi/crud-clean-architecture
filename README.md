## crud_clean_architecture

- 🇺🇸 English (current)
- 🇷🇺 [Русский](docs/readme/ru.md)

This project provides a lightweight architecture for CRUD-oriented services and microservices.

The architecture was created to solve a common problem found in many modern backend projects: the gap between overly simplistic CRUD applications and fully implemented Domain-Driven Design (DDD) solutions.

In many projects, a traditional layered architecture quickly becomes tightly coupled to specific technologies such as FastAPI, SQLAlchemy, PostgreSQL, Redis, or Kafka. As the project grows, replacing infrastructure components often requires modifications across the business layer.

On the other hand, adopting a complete DDD approach introduces additional complexity through concepts such as:

* Domain Entities
* Aggregates
* Value Objects
* Domain Events
* Use Cases
* Application Services

While these patterns are powerful, they are often unnecessary for typical CRUD systems and small to medium-sized microservices.

This architecture aims to provide a pragmatic middle ground.

### Design Goals

* Keep business logic independent from frameworks and infrastructure.
* Allow replacing databases without changing business rules.
* Allow replacing external services and message brokers.
* Allow replacing transport layers such as REST, gRPC, or GraphQL.
* Keep endpoints thin and focused on request handling.
* Avoid unnecessary abstractions and boilerplate.
* Remain simple enough for everyday CRUD development.

### Core Concepts

The architecture is built around a small set of concepts:

* API Layer
* DTOs
* Services
* Ports
* Infrastructure

### Architectural Philosophy

Business logic should not know:

* which web framework is being used;
* which database is being used;
* how authentication is implemented;
* how external services are integrated.

The application should depend on contracts (Ports), while infrastructure provides concrete implementations.

```text
API
 ↓
DTO
 ↓
Service
 ↓
Port
 ↑
Infrastructure
```

This approach keeps the system easy to understand while preserving the flexibility needed for long-term maintenance and growth.

The result is a clean, technology-independent application core without the overhead of a full DDD architecture.

### 1. API Слой (API Layer)
- **Расположение**: `./crud_clean_architecture/api/v1/` - основные route-ы
- **Описание**: Основной интерфейс взаимодействия с системой через REST API
- **Примеры файлов**: 
  - `users.py` - управление пользователями
  - `bloggers.py` - управление блогерами
  - `appeals.py` - управление обращениями
  - `system_administration.py` - административные функции
  - `node_connected.py` - обработка подключения узлов

### 2. Обработчики зависимостей (Dependencies)
- **Расположение**: `./crud_clean_architecture/api/dependencies/`
- **Описание**: Слой, отвечающий за предоставление зависимостей для обработчиков запросов
- **Примеры файлов**: 
  - `users_dep.py` - зависимости для пользователей
  - `bloggers_dep.py` - зависимости для блогеров
  - `appeals_dep.py` - зависимости для обращений
  - `consumers_dep.py` - зависимости для потребителей
  - `system_administration_dep.py` - зависимости для системной администрации

### 3. Схемы (Schemas)
- **Расположение**: `./crud_clean_architecture/api/schemas/`
- **Описание**: Схемы данных в формате Pydantic для валидации входных и выходных данных API
- **Примеры файлов**:
  - `auth_schemas.py` - схемы для аутентификации
  - `user_schemas.py` - схемы для пользователей
  - `blogger_schemas.py` - схемы для блогеров
  - `appeal_schemas.py` - схемы для обращений
  - `consumer_schemas.py` - схемы для потребителей
  - `admin_schemas.py` - схемы для администраторов

### 4. Слой сервисов (Services Layer)
- **Расположение**: `./crud_clean_architecture/services/`
- **Описание**: Бизнес-логика приложения, реализует основные функции системы
- **Примеры файлов**:
  - `users/` - сервис пользователей
  - `bloggers/` - сервис блогеров
  - `appeals/` - сервис обращений
  - `admins/` - сервис администраторов
  - `consumers/` - сервис потребителей
  - `security/` - сервис безопасности
  - `marks/` - сервис оценок (marks)
  - `youtube_content/` - сервис контента YouTube

### 5. Репозитории (Repositories Layer)
- **Расположение**: `./crud_clean_architecture/infrastructure/repositories/`
- **Описание**: Слой доступа к данным, реализует паттерн Repository для взаимодействия с БД
- **Примеры файлов**:
  - `user_repository.py` - репозиторий пользователей
  - `blogger_repository.py` - репозиторий блогеров
  - `appeal_repository.py` - репозиторий обращений
  - `admin_repository.py` - репозиторий администраторов
  - `consumer_repository.py` - репозиторий потребителей
  - `youtube_repository.py` - репозиторийYoutube контента

### 6. Провайдеры (Providers Layer)
- **Расположение**: `./crud_clean_architecture/infrastructure/providers/`
- **Описание**: Слой провайдеров, реализует внешние зависимости и инструменты
- **Примеры файлов**:
  - `password_hasher_providers.py` - провайдеры хеширования паролей
  - `token_providers.py` - провайдеры токенов
  - `api_key_providers.py` - провайдеры API ключей  
  - `link_detector_providers.py` - провайдеры детектора ссылок
  - `queue_providers.py` - провайдеры очередей

### 7. Инфрастрктура (Infrastructure Layer)
- **Расположение**: `./crud_clean_architecture/infrastructure/`
- **Описание**: Общие инфраструктурные компоненты
- **Примеры файлов**:
  - `sql_db/` - конфигурация и сессии БД
  - `providers/` - провайдеры зависимостей
  - `repositories/` - реализации репозиториев

### 8. База данных (Database)
- **Расположение**: `./crud_clean_architecture/infrastructure/sql_db/migrations/`
- **Описание**: Миграции базы данных и настройки подключения
- **Примеры файлов**:
  - `session.py` - сессия SQLAlchemy
  - `migrations/` - папка с миграциями

### 9. Модели (Data Models)
- **Расположение**: `./crud_clean_architecture/infrastructure/sql_db/schemes/entity_schemas/`
- **Описание**: Схемы данных для таблиц базы данных (модели ORM)
- **Примеры файлов**:
  - `user_scheme.py` - схема пользователя
  - `blogger_scheme.py` - схема блогера  
  - `appeal_schema.py` - схема обращения
  - `admin_scheme.py` - схема администратора
  - `consumer_scheme.py` - схема потребителя
  - `data_request_scheme.py` - схема запроса данных
  - `relations_scheme.py` - схемы отношений
  - `content/youtube_scheme.py` - схема YouTube контента

### 10. Примитивные сущности (Base Schemes)
- **Расположение**: `./crud_clean_architecture/infrastructure/sql_db/schemes/base_scheme.py`
- **Описание**: Базовый класс для моделей

### 11. Приложение (Application Layer)
- **Расположение**: `./crud_clean_architecture/application/`
- **Описание**: Логика запуска приложения, инициализация и конфигурация
- **Файлы**:
  - `factory.py` - фабрика для создания приложения
  - `startup.py` - логика запуска приложения

### 12. Конфигурации
- **Расположение**: `./crud_clean_architecture/settings.py`
- **Описание**: Конфигурационный файл приложения


Дополнительно также присутствуют файлы конфигурации:
- `Dockerfile` - конфигурация Docker контейнера  
- `docker-compose.yml` - описания композиции контейнеров
- `alembic.ini` - настройки Alembic миграций
- `pyproject.toml` - конфигурация проекта Python

## Связь с внешними системами

Система использует следующие технологии:
- **FastAPI** - для создания REST API 
- **SQLAlchemy** - ORM для взаимодействия с БД
- **Alembic** - системы миграций БД
- **JWT** - аутентификация через токены
- **Python 3.13** - основной язык реализации

## Описание файлов проекта

### Главные файлы:

- `crud_clean_architecture/start_server.py` - точка входа в приложение
- `crud_clean_architecture/settings.py` - конфигурация системы
- `alembic.ini` - настройки миграций БД
- `Dockerfile` и `docker-compose.yml` - для контейнеризации приложения

### Основные папки:

- `crud_clean_architecture/api/` - API интерфейсы
- `crud_clean_architecture/services/` - бизнес-логика
- `crud_clean_architecture/infrastructure/` - инфраструктура и поддержка
- `tests/` - тесты, включая тесты для сервисов оценок (marks)