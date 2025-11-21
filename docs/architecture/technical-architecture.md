# Technical Architecture - Electronics Store for Programmers

## Architecture Overview

The Electronics Store for Programmers follows Clean Architecture principles combined with Domain-Driven Design (DDD) tactical patterns to create a maintainable, testable, and scalable e-commerce platform.

### Architectural Principles

1. **Separation of Concerns**: Each layer has distinct responsibilities
2. **Dependency Inversion**: Dependencies point inward toward the domain
3. **Technology Independence**: Business logic is framework-agnostic
4. **Testability**: High test coverage through dependency injection
5. **Scalability**: Horizontal scaling capability with stateless services

---

## System Architecture Layers

### 1. Domain Layer (Core Business Logic)

**Purpose**: Contains enterprise business rules and domain logic independent of any framework or technology.

**Components**:

- **Entities**: User, Product, Order, Cart, Review, Discount, BlogPost
- **Value Objects**: Money, Email, Address, Password, ProductSpecification, Rating
- **Aggregates**:
  - User (root) → Address collection
  - Product (root) → ProductImage collection, ProductSpecification collection
  - Order (root) → OrderItem entities, ShippingAddress, Payment
  - Cart (root) → CartItem entities
- **Domain Services**: PricingService, InventoryService, DiscountCalculator
- **Repository Interfaces**: IUserRepository, IProductRepository, IOrderRepository, ICartRepository
- **Domain Events**: UserRegistered, OrderPlaced, PaymentProcessed, ProductReviewed

**Key Characteristics**:

- Zero dependencies on external frameworks
- Pure Python classes with business logic
- Immutable value objects
- Aggregate boundaries enforce consistency

---

### 2. Application Layer (Use Cases)

**Purpose**: Orchestrates domain objects to fulfill application-specific business rules. Contains use cases that implement user stories.

**Components**:

- **Use Cases (Commands)**:
  - User: RegisterUser, LoginUser, UpdateUserProfile, ResetPassword
  - Product: CreateProduct, UpdateProduct, DeleteProduct, GetProductList, SearchProducts
  - Cart: AddToCart, UpdateCartItem, RemoveFromCart, GetCart
  - Order: CreateOrder, ProcessPayment, GetOrderHistory, GetOrderDetails
  - Review: CreateReview, GetProductReviews
- **DTOs**: UserDTO, ProductDTO, OrderDTO, CartDTO (for API request/response)
- **Application Services**: UserService, ProductService, OrderService, CartService
- **Interfaces**: IEmailService, IPaymentService, IStorageService
- **Specifications**: ProductSpecification (for complex queries)

**Key Characteristics**:

- Thin orchestration layer
- Depends only on domain layer
- Defines interfaces for external services
- Returns DTOs, not domain entities

---

### 3. Infrastructure Layer (Technical Implementation)

**Purpose**: Provides concrete implementations of interfaces defined in application layer. Contains framework-specific code.

**Components**:

- **Database**:
  - SQLAlchemy models (ORM mappings)
  - Repository implementations
  - Alembic migrations
  - Database connection pooling
- **External Services**:
  - Stripe payment service implementation
  - PayPal payment service implementation
  - SendGrid email service implementation
  - AWS S3 storage service implementation
- **Caching**:
  - Redis cache implementation
  - Cache decorators for repositories
- **Logging & Monitoring**:
  - Structured logging with Python logging
  - Prometheus metrics
  - Sentry error tracking

**Key Characteristics**:

- Implements application layer interfaces
- Contains all external dependencies
- Technology-specific implementations
- Easily replaceable (e.g., swap S3 for local storage in tests)

**Example**:

```python
# infrastructure/repositories/order_repository.py
class SqlAlchemyOrderRepository(IOrderRepository):
    def __init__(self, db_session: AsyncSession):
        self._session = db_session

    async def save(self, order: Order) -> None:
        order_model = OrderModel.from_entity(order)
        self._session.add(order_model)
        await self._session.commit()

    async def get_by_id(self, order_id: OrderId) -> Optional[Order]:
        result = await self._session.execute(
            select(OrderModel).where(OrderModel.id == order_id.value)
        )
        order_model = result.scalar_one_or_none()
        return order_model.to_entity() if order_model else None
```

---

### 4. Presentation Layer (API & UI)

**Purpose**: Handles HTTP requests, authentication, validation, and response formatting. Entry point for all external interactions.

**Components**:

**Backend (FastAPI)**:

- REST API endpoints (routers)
- Request/response models (Pydantic)
- Authentication middleware (JWT)
- Input validation
- Error handling
- OpenAPI/Swagger documentation
- Rate limiting
- CORS configuration

**Frontend (React)**:

- React components (pages, UI components)
- Redux Toolkit store (state management)
- API client (Axios)
- React Router (navigation)
- Form handling and validation
- Error boundaries

**Key Characteristics**:

- Thin layer that delegates to application services
- Converts between HTTP and application DTOs
- Handles cross-cutting concerns (auth, logging, rate limiting)
- No business logic

**Backend Example**:

---

## API Design

### RESTful Conventions

- **Resources**: Nouns (plural) for endpoints (e.g., `/products`, `/orders`)
- **HTTP Methods**: GET (read), POST (create), PUT/PATCH (update), DELETE (delete)
- **Status Codes**: 200 (OK), 201 (Created), 204 (No Content), 400 (Bad Request), 401 (Unauthorized), 403 (Forbidden), 404 (Not Found), 500 (Internal Server Error)
- **Pagination**: Query parameters `?page=1&per_page=24`
- **Filtering**: Query parameters `?category=keyboards&price_min=50&price_max=200`
- **Sorting**: Query parameter `?sort=price_asc` or `?sort=rating_desc`

### Example Endpoints

```
# Authentication
POST   /api/v1/auth/register
POST   /api/v1/auth/login
POST   /api/v1/auth/logout
POST   /api/v1/auth/refresh
POST   /api/v1/auth/password-reset-request
POST   /api/v1/auth/password-reset

# Products
GET    /api/v1/products                 # List with pagination/filtering
GET    /api/v1/products/{id}            # Get details
GET    /api/v1/products/{id}/reviews    # Get product reviews
POST   /api/v1/products                 # Admin: Create product
PUT    /api/v1/products/{id}            # Admin: Update product
DELETE /api/v1/products/{id}            # Admin: Delete product

# Cart
GET    /api/v1/cart                     # Get current user's cart
POST   /api/v1/cart/items               # Add item to cart
PATCH  /api/v1/cart/items/{id}          # Update quantity
DELETE /api/v1/cart/items/{id}          # Remove from cart

# Orders
GET    /api/v1/orders                   # Get user's order history
GET    /api/v1/orders/{id}              # Get order details
POST   /api/v1/orders                   # Create order (checkout)

# Wishlist
GET    /api/v1/wishlist                 # Get wishlist
POST   /api/v1/wishlist/items           # Add to wishlist
DELETE /api/v1/wishlist/items/{id}      # Remove from wishlist

# Reviews
POST   /api/v1/reviews                  # Create review
GET    /api/v1/reviews/{id}             # Get review details

# Admin
GET    /api/v1/admin/products           # List all products
GET    /api/v1/admin/orders             # List all orders
PATCH  /api/v1/admin/orders/{id}/status # Update order status
GET    /api/v1/admin/analytics          # Get analytics data
```

---

## Security Architecture

### Authentication & Authorization

- **JWT Tokens**: Stateless authentication with access tokens (24h expiration)
- **Refresh Tokens** (Phase 2): Long-lived tokens for obtaining new access tokens
- **Password Security**: bcrypt hashing with cost factor 12
- **Role-Based Access Control (RBAC)**: User roles (customer, admin) enforced at API level

### Data Security

- **Input Validation**: Pydantic models validate all API inputs
- **SQL Injection Prevention**: SQLAlchemy parameterized queries
- **XSS Prevention**: React automatic escaping, CSP headers
- **CSRF Protection**: CSRF tokens for state-changing operations (cookies)
- **HTTPS Enforcement**: TLS 1.3, HSTS headers
- **Secure Cookies**: httpOnly, secure, sameSite flags for auth cookies

### Rate Limiting

- **Authentication endpoints**: 5 requests per 15 minutes per IP
- **API endpoints**: 100 requests per minute per user
- **Admin endpoints**: 200 requests per minute per admin

### Payment Security

- **PCI DSS Compliance**: Using Stripe/PayPal (card data never touches our servers)
- **Payment Idempotency**: Prevent double-charging with idempotency keys
- **Fraud Detection**: Stripe Radar for fraud prevention

---

## Performance & Scalability

### Caching Strategy

- **Redis Caching**:
  - Product catalog (5-minute TTL)
  - User sessions (24-hour TTL)
  - API response caching for frequently accessed data
  - Rate limiting counters

### Database Optimization

- **Indexes**: On foreign keys, frequently queried fields (email, SKU, category_id)
- **Connection Pooling**: SQLAlchemy async connection pool (min 5, max 20)
- **Query Optimization**: Eager loading for relationships, avoid N+1 queries
- **Read Replicas** (future): For analytics and reporting queries

### Frontend Optimization

- **Code Splitting**: React lazy loading for routes
- **Image Optimization**: Responsive images, WebP format, lazy loading
- **CDN**: CloudFront for static assets
- **Bundle Optimization**: Vite tree-shaking, minification

### Horizontal Scaling

- **Stateless API**: No session state in application servers
- **Load Balancer**: AWS ALB distributing traffic across multiple EC2 instances
- **Database**: Vertical scaling initially, read replicas for future growth
- **Cache**: Redis cluster for high availability

---

**Document Owner**: Tech Lead  
**Contributors**: Backend Engineer, Frontend Engineer  
**Last Updated**: November 12, 2025  
**Version**: 1.0.0  
**Status**: ✅ Approved - Implementation Guide
