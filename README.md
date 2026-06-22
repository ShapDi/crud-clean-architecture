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
