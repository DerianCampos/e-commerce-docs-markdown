# Dependencies - Electronics Store for Programmers

This document tracks all technical dependencies, third-party services, infrastructure requirements, and inter-feature dependencies for the project.

---

## Technical Dependencies

### Backend Dependencies (Python)

#### Core Framework & Web

```
fastapi==0.104.1          # Web framework
uvicorn[standard]==0.24.0 # ASGI server
pydantic==2.5.0           # Data validation
python-multipart==0.0.6   # File upload support
```

#### Database & ORM

```
sqlalchemy[asyncio]==2.0.23   # ORM with async support
asyncpg==0.29.0               # PostgreSQL async driver
alembic==1.12.1               # Database migrations
psycopg2-binary==2.9.9        # PostgreSQL adapter (sync, for migrations)
```

#### Authentication & Security

```
python-jose[cryptography]==3.3.0  # JWT tokens
passlib[bcrypt]==1.7.4           # Password hashing
python-multipart==0.0.6           # OAuth2 password flow
bcrypt==4.1.1                     # Password hashing algorithm
```

#### Testing

```
pytest==7.4.3
pytest-asyncio==0.21.1
pytest-cov==4.1.0
httpx==0.25.2                  # Async HTTP client for testing
faker==20.1.0                  # Test data generation
```

#### Payment Processing

```
stripe==7.6.0               # Stripe payment integration
paypalrestsdk==1.13.1       # PayPal SDK (Phase 1)
```

#### Email & Communication

```
sendgrid==6.11.0           # Email service
python-sendgrid==6.11.0    # Python wrapper
```

#### Caching & Sessions

```
redis==5.0.1                   # Redis client
hiredis==2.2.3                 # C parser for performance
aioredis==2.0.1                # Async Redis (if needed)
```

#### AWS & Cloud Services

```
boto3==1.29.7              # AWS SDK
botocore==1.32.7           # AWS core functionality
```

#### Monitoring & Logging

```
prometheus-client==0.19.0  # Prometheus metrics
sentry-sdk==1.38.0         # Error tracking
python-json-logger==2.0.7  # Structured logging
```

#### Utilities

```
python-dotenv==1.0.0       # Environment variables
python-dateutil==2.8.2     # Date utilities
pytz==2023.3               # Timezone support
```

### Frontend Dependencies (npm/yarn)

#### Core Framework

```json
{
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "typescript": "^5.2.2",
  "vite": "^5.0.0"
}
```

#### UI Library

```json
{
  "antd": "^5.11.5",
  "@ant-design/icons": "^5.2.6"
}
```

#### State Management

```json
{
  "@reduxjs/toolkit": "^1.9.7",
  "react-redux": "^8.1.3",
  "redux-persist": "^6.0.0"
}
```

#### Routing

```json
{
  "react-router-dom": "^6.20.0"
}
```

#### API & Data Fetching

```json
{
  "axios": "^1.6.2",
  "@tanstack/react-query": "^5.12.2"
}
```

#### Forms & Validation

```json
{
  "react-hook-form": "^7.48.2",
  "zod": "^3.22.4",
  "@hookform/resolvers": "^3.3.2"
}
```

#### Styling

```json
{
  "sass": "^1.69.5",
  "classnames": "^2.3.2"
}
```

#### Testing

```json
{
  "vitest": "^1.0.4",
  "@testing-library/react": "^14.1.2",
  "@testing-library/jest-dom": "^6.1.5",
  "@testing-library/user-event": "^14.5.1"
}
```

#### Build Tools

```json
{
  "@vitejs/plugin-react": "^4.2.1",
  "vite-plugin-compression": "^0.5.1",
  "vite-plugin-pwa": "^0.17.4"
}
```

#### Utilities

```json
{
  "date-fns": "^2.30.0",
  "lodash": "^4.17.21",
  "react-helmet-async": "^2.0.4"
}
```

---

## Infrastructure Dependencies

### Required Services

#### AWS Services

- **EC2**: Application servers (t3.medium initially, scale to t3.large)
- **RDS PostgreSQL**: Database (db.t3.micro for MVP, db.t3.small for production)
- **S3**: Static asset storage (product images, user uploads)
- **CloudFront**: CDN for static content delivery
- **ElastiCache (Redis)**: Caching layer (cache.t3.micro for MVP)
- **Route 53**: DNS management
- **Certificate Manager (ACM)**: SSL/TLS certificates
- **CloudWatch**: Logging and basic monitoring

#### External Services

##### Payment Processing

- **Stripe**: Primary payment gateway (PCI DSS compliant)
  - Account setup required before MVP Sprint 3
  - Test and production API keys
  - Webhook endpoints for payment events
- **PayPal**: Alternative payment method (Phase 1)
  - Business account required
  - OAuth credentials

##### Email Delivery

- **SendGrid**: Transactional email service
  - Free tier: 100 emails/day (sufficient for MVP)
  - Account verification required
  - API key and sender domain verification

##### Monitoring & Error Tracking

- **Sentry**: Error tracking and performance monitoring
  - Free tier suitable for MVP
  - DSN configuration required
- **Prometheus + Grafana**: Metrics and dashboards
  - Self-hosted on EC2 or dedicated server

##### Live Chat (Phase 3)

- **Crisp** or **Intercom**: Customer support chat
  - Account setup required in Phase 3
  - Widget integration

##### Analytics (Phase 2)

- **Google Analytics 4**: User behavior tracking
  - Property setup required
  - Tracking ID configuration

---

## Development Environment Dependencies

### Required Software

#### Backend Development

- Python 3.11+
- PostgreSQL 15+ (local development)
- Redis 7+ (local development)
- Docker & Docker Compose
- Git
- Postman or similar (API testing)

#### Frontend Development

- Node.js 18+ LTS
- npm or yarn
- Git
- VS Code (recommended) with extensions:
  - ESLint
  - Prettier
  - TypeScript
  - React snippets

#### DevOps

- AWS CLI
- Docker
- kubectl (future Kubernetes deployment)
- Terraform (future IaC)

---

## Inter-Feature Dependencies

### MVP Features

#### User Authentication (Epic 1)

- **Depends on**: None (foundational)
- **Blocks**: All user-specific features (cart, orders, wishlist, reviews, admin)

#### Product Catalog (Epic 2)

- **Depends on**: None (foundational)
- **Blocks**: Cart, Wishlist, Search, Reviews, Admin product management

#### Shopping Cart (Epic 3)

- **Depends on**: User Authentication, Product Catalog
- **Blocks**: Checkout

#### Checkout & Payment (Epic 5)

- **Depends on**: Shopping Cart, User Authentication
- **Blocks**: Order Management

#### Order Management (Epic 6)

- **Depends on**: Checkout & Payment
- **Blocks**: Admin order management, Returns (Phase 2)

#### Admin Dashboard (Epic 8 - Basic)

- **Depends on**: User Authentication (admin role)
- **Blocks**: Advanced admin features (Phase 2)

### Phase 1 Features

#### Advanced Search & Filters (Epic 2)

- **Depends on**: MVP Product Catalog
- **Blocks**: Enhanced product discovery

#### Wishlist (Epic 4)

- **Depends on**: User Authentication, Product Catalog
- **Blocks**: None (independent feature)

#### Product Reviews (Epic 7)

- **Depends on**: User Authentication, Product Catalog, Order Management (verified purchase)
- **Blocks**: Review moderation (admin)

#### PayPal Integration (Epic 5)

- **Depends on**: MVP Checkout flow
- **Blocks**: None (parallel payment method)

### Phase 2 Features

#### Admin Dashboard - Full (Epic 8)

- **Depends on**: MVP Admin, Order Management
- **Blocks**: Analytics, Inventory management

#### Promotions & Discounts (Epic 9)

- **Depends on**: MVP Checkout, Admin Dashboard
- **Blocks**: Flash sales, Discount campaigns

#### Blog Platform (Epic 10)

- **Depends on**: Admin Dashboard (content management)
- **Blocks**: SEO traffic, Content marketing

#### Order Tracking (Epic 6)

- **Depends on**: MVP Order Management, Admin Dashboard
- **Blocks**: Shipment notifications

### Phase 3 Features

#### Personalization (Epic 11)

- **Depends on**: User browsing data, Order history
- **Blocks**: Targeted recommendations

#### Live Chat (Epic 12)

- **Depends on**: None (independent integration)
- **Blocks**: None

#### SEO Optimization

- **Depends on**: Product Catalog, Blog Platform
- **Blocks**: Organic traffic growth

#### Monitoring & Observability

- **Depends on**: None (infrastructure)
- **Blocks**: Production readiness

---

## Version Compatibility Matrix

| Component  | Minimum Version | Recommended Version | Notes                    |
| ---------- | --------------- | ------------------- | ------------------------ |
| Python     | 3.11            | 3.11+               | Type hints, performance  |
| PostgreSQL | 14              | 15+                 | Performance improvements |
| Redis      | 6               | 7+                  | Better memory efficiency |
| Node.js    | 18 LTS          | 20 LTS              | Frontend build           |
| React      | 18              | 18.2+               | Concurrent features      |
| TypeScript | 5.0             | 5.2+                | Latest type features     |
| FastAPI    | 0.100           | 0.104+              | Latest async features    |
| SQLAlchemy | 2.0             | 2.0+                | Async ORM support        |

---

## Dependency Management

### Backend (Python)

**Development**:

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# or
venv\Scripts\activate  # Windows

# Install dependencies
pip install -r requirements.txt

# Install dev dependencies
pip install -r requirements-dev.txt
```

**Production**:

```bash
pip install --no-cache-dir -r requirements.txt
```

**Update Dependencies**:

```bash
pip list --outdated
pip install --upgrade <package-name>
pip freeze > requirements.txt
```

### Frontend (npm/yarn)

**Development**:

```bash
npm install
# or
yarn install
```

**Production Build**:

```bash
npm run build
# or
yarn build
```

**Update Dependencies**:

```bash
npm outdated
npm update <package-name>
# or
yarn outdated
yarn upgrade <package-name>
```

---

## Security Considerations

### Dependency Security

1. **Automated Scanning**:

   - GitHub Dependabot alerts enabled
   - npm audit / pip audit in CI/CD
   - Regular dependency updates

2. **Pinned Versions**:

   - Use exact versions in requirements.txt and package.json
   - Test updates in development before production

3. **Vulnerability Management**:
   - Review and patch critical vulnerabilities immediately
   - Evaluate medium/low vulnerabilities for risk
   - Document accepted risks for dependencies with no fixes

---

## Deprecation & Migration Plan

### Upcoming Changes

1. **Python 3.11 → 3.12** (Q2 2026):

   - Performance improvements
   - New type hint features
   - Test compatibility in Q1 2026

2. **React 18 → 19** (When stable):

   - New concurrent features
   - Migration guide review
   - Test in development environment first

3. **SQLAlchemy 2.0 → 2.1** (As released):
   - Monitor release notes
   - Test migrations
   - Update ORM usage patterns

---

## Installation Scripts

### Backend Setup

```bash
#!/bin/bash
# setup_backend.sh

echo "Setting up backend environment..."

# Create virtual environment
python3.11 -m venv venv
source venv/bin/activate

# Upgrade pip
pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Setup database
alembic upgrade head

# Create .env file if not exists
if [ ! -f .env ]; then
    cp .env.example .env
    echo "Created .env file. Please update with your credentials."
fi

echo "Backend setup complete!"
```

### Frontend Setup

```bash
#!/bin/bash
# setup_frontend.sh

echo "Setting up frontend environment..."

# Check Node version
NODE_VERSION=$(node -v)
echo "Node version: $NODE_VERSION"

# Install dependencies
npm install

# Create .env file if not exists
if [ ! -f .env ]; then
    cp .env.example .env
    echo "Created .env file. Please update with your API URL."
fi

echo "Frontend setup complete!"
```

---

**Document Owner**: Tech Lead  
**Contributors**: Backend Engineer, Frontend Engineer  
**Last Updated**: November 12, 2025  
**Version**: 1.0.0  
**Status**: ✅ Complete - Living Document (update as dependencies change)
