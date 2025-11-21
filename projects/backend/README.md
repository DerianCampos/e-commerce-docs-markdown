# Electronics Store - Backend API

FastAPI-based backend for the Electronics Store for Programmers e-commerce platform, following Clean Architecture and Domain-Driven Design principles.

## Technology Stack

- **Framework**: FastAPI 0.104+
- **Language**: Python 3.11+
- **Database**: PostgreSQL 15+
- **ORM**: SQLAlchemy 2.0+ (async)
- **Migrations**: Alembic
- **Authentication**: JWT (python-jose)
- **Validation**: Pydantic 2.0+
- **Testing**: Pytest + pytest-asyncio
- **Caching**: Redis 7+
- **Payments**: Stripe SDK, PayPal SDK
- **Email**: SendGrid
- **Storage**: AWS S3 (boto3)

## Project Structure

```
backend/
├── src/
│   ├── domain/              # Domain layer (entities, value objects, aggregates)
│   │   ├── aggregates/
│   │   │   ├── user.py
│   │   │   ├── product.py
│   │   │   ├── order.py
│   │   │   └── cart.py
│   │   ├── value_objects/
│   │   │   ├── money.py
│   │   │   ├── email.py
│   │   │   └── address.py
│   │   ├── repositories/    # Repository interfaces
│   │   └── events/          # Domain events
│   ├── application/         # Application layer (use cases, DTOs, interfaces)
│   │   ├── use_cases/
│   │   ├── dtos/
│   │   ├── interfaces/
│   │   └── services/
│   ├── infrastructure/      # Infrastructure layer (implementations)
│   │   ├── database/
│   │   │   ├── models/      # SQLAlchemy models
│   │   │   └── repositories/ # Repository implementations
│   │   ├── external_services/
│   │   │   ├── stripe_service.py
│   │   │   ├── email_service.py
│   │   │   └── storage_service.py
│   │   └── cache/
│   ├── presentation/        # Presentation layer (FastAPI routes, middleware)
│   │   ├── api/
│   │   │   ├── routers/
│   │   │   ├── middleware/
│   │   │   └── dependencies.py
│   │   └── schemas/         # Pydantic request/response schemas
│   ├── config.py            # Configuration management
│   └── main.py              # FastAPI application entry point
├── tests/
│   ├── unit/
│   ├── integration/
│   └── conftest.py
├── alembic/                 # Database migrations
├── requirements.txt
├── requirements-dev.txt
├── .env.example
├── .gitignore
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## Prerequisites

- Python 3.11 or higher
- PostgreSQL 15 or higher
- Redis 7 or higher
- AWS account (for S3)
- Stripe account (for payments)
- SendGrid account (for email)

**Maintainers**: Backend Engineering Team  
**Last Updated**: November 12, 2025
