# Vendora — System Architecture

**Document:** System Architecture  
**Version:** 1.0  
**Status:** Initial Draft  
**Project:** Vendora  
**Repository:** `vendora-multi-vendor-ecommerce`

---

# 1. Overview

Vendora is a **Multi-Vendor E-Commerce Marketplace** designed to support multiple sellers who can list and manage products while customers can browse products, manage carts, place orders, make payments, and track deliveries.

The platform will provide separate capabilities for:

- Customers
- Sellers
- Administrators
- Marketplace operations
- AI-powered assistants

The initial system will use a **Modular Monolith Architecture**.

The architecture will be designed with clear module boundaries so that selected modules can be extracted into independent services in the future if the system requires microservices.

---

# 2. Architecture Goals

The architecture is designed around the following goals:

1. Maintain clear separation between business domains.
2. Keep the initial system simple to develop and deploy.
3. Support future horizontal scaling.
4. Protect critical business operations.
5. Keep business logic independent from external providers.
6. Support automated testing.
7. Provide clear API boundaries.
8. Make future microservice extraction possible.
9. Support AI functionality without making AI a dependency for core e-commerce operations.
10. Provide maintainable and extensible project structure.

---

# 3. High-Level Architecture

The initial architecture will follow this structure:

```text
                         +----------------------+
                         |      Customers       |
                         |      Sellers         |
                         |      Admins          |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         |    React Frontend    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         |      FastAPI API     |
                         |       Gateway        |
                         +----------+-----------+
                                    |
             +----------------------+----------------------+
             |                      |                      |
             v                      v                      v
      +-------------+        +-------------+        +-------------+
      |    Users    |        |   Catalog   |        |    Cart     |
      |    Auth     |        |  Products   |        |            |
      +-------------+        +-------------+        +-------------+
             |                      |                      |
             +----------------------+----------------------+
                                    |
                                    v
                            +---------------+
                            |    Orders     |
                            +-------+-------+
                                    |
                    +---------------+---------------+
                    |               |               |
                    v               v               v
             +------------+   +------------+   +------------+
             |  Payments  |   | Inventory  |   |  Shipping  |
             +------------+   +------------+   +------------+
                    |
                    v
             +-------------+
             |Notifications|
             +-------------+

                                    |
                                    v
                         +----------------------+
                         |      Database        |
                         +----------------------+

                                    |
                                    v
                         +----------------------+
                         |    AI Assistants     |
                         | Support / Product    |
                         +----------------------+
```

---

# 4. Initial Architecture Style

Vendora will initially use a:

**Modular Monolith**

This means the application will run as a single deployable backend application while internally being divided into independent business modules.

Example:

```text
FastAPI Application
        |
        +-- Users
        +-- Products
        +-- Categories
        +-- Cart
        +-- Orders
        +-- Payments
        +-- Coupons
        +-- Reviews
        +-- Wishlist
        +-- Inventory
        +-- Shipping
        +-- Notifications
        +-- Chatbots
```

The modules should have clear responsibilities and interfaces.

---

# 5. Why Modular Monolith First

The first version will not immediately use microservices.

A modular monolith provides:

- Simpler development
- Easier debugging
- Easier local development
- Easier deployment
- Lower infrastructure complexity
- Easier database transactions
- Faster initial development

At the same time, clear module boundaries will prepare the application for future service extraction.

---

# 6. Frontend Architecture

The customer-facing frontend will use:

**React**

The frontend will communicate with the backend through REST APIs.

High-level structure:

```text
React Application
       |
       +-- Authentication
       +-- Home
       +-- Product Catalog
       +-- Product Details
       +-- Search
       +-- Cart
       +-- Checkout
       +-- Orders
       +-- Wishlist
       +-- Reviews
       +-- Seller Features
       +-- Admin Features
       +-- AI Assistants
```

---

# 7. Frontend State Management

The frontend will separate server state from local application state.

Recommended approach:

```text
React
 |
 +-- TanStack Query
 |       |
 |       +-- Products
 |       +-- Categories
 |       +-- Orders
 |       +-- Reviews
 |       +-- Seller Data
 |
 +-- Zustand / Redux Toolkit
         |
         +-- Authentication State
         +-- Cart State
         +-- UI State
```

The final state-management library can be selected during frontend implementation.

---

# 8. Backend Architecture

The backend will use:

**FastAPI + Python**

The backend will expose REST APIs to the frontend.

High-level request flow:

```text
Client
   |
   v
FastAPI Router
   |
   v
Schema Validation
   |
   v
Authentication / Authorization
   |
   v
Service Layer
   |
   v
Repository / Data Access Layer
   |
   v
Database
```

---

# 9. Backend Layer Responsibilities

Each backend module should follow a layered structure.

Example:

```text
products/
├── router.py
├── schemas.py
├── models.py
├── service.py
├── repository.py
└── dependencies.py
```

---

## 9.1 Router Layer

The router layer is responsible for:

- HTTP endpoints
- Request handling
- Authentication dependencies
- Input/output schemas
- HTTP status codes

The router should not contain complex business logic.

---

## 9.2 Schema Layer

The schema layer will use Pydantic models.

Responsibilities include:

- Request validation
- Response validation
- Serialization
- Data format definitions

---

## 9.3 Service Layer

The service layer contains business logic.

Examples:

- Create product
- Add item to cart
- Calculate order totals
- Apply coupon
- Create order
- Cancel order
- Update inventory

Business rules should primarily exist in this layer.

---

## 9.4 Repository Layer

The repository layer handles database access.

Responsibilities include:

- Queries
- Inserts
- Updates
- Deletes
- Database-specific operations

This helps keep database access separate from business logic.

---

# 10. Core Modules

The backend will contain the following primary modules:

```text
modules/
├── users/
├── products/
├── categories/
├── cart/
├── orders/
├── payments/
├── coupons/
├── reviews/
├── wishlist/
├── inventory/
├── shipping/
└── notifications/
```

---

# 11. Users Module

The Users module manages:

- Customer accounts
- Seller accounts
- Administrator accounts
- Authentication
- Authorization
- Profiles
- Addresses
- Roles

Example roles:

```text
Customer
Seller
Admin
```

The module will provide the authentication foundation for the rest of the application.

---

# 12. Products Module

The Products module manages product information.

Responsibilities include:

- Product creation
- Product updates
- Product deletion
- Product details
- Product pricing
- Product images
- Product status
- Seller ownership

Example:

```text
Seller
   |
   +-- Product
   |     |
   |     +-- Name
   |     +-- Description
   |     +-- Price
   |     +-- Images
   |     +-- Category
   |     +-- Inventory
   |
   +-- Product
```

---

# 13. Categories Module

The Categories module manages product classification.

Responsibilities include:

- Category creation
- Category updates
- Category hierarchy
- Category assignment
- Category listing

Example:

```text
Electronics
 |
 +-- Mobiles
 |
 +-- Laptops
 |
 +-- Cameras
```

---

# 14. Cart Module

The Cart module manages customer shopping carts.

Responsibilities include:

- Add product
- Remove product
- Update quantity
- View cart
- Calculate subtotal
- Validate product availability

Example:

```text
Customer
   |
   v
  Cart
   |
   +-- Product A
   +-- Product B
   +-- Product C
```

---

# 15. Orders Module

The Orders module manages the order lifecycle.

Responsibilities include:

- Order creation
- Order items
- Order status
- Order cancellation
- Order history
- Order totals

Example lifecycle:

```text
Pending
   |
   v
Confirmed
   |
   v
Processing
   |
   v
Shipped
   |
   v
Delivered
```

Possible cancellation flow:

```text
Pending
   |
   v
Cancelled
```

---

# 16. Payments Module

The Payments module handles payment-related business logic.

The first implementation may use a dummy payment gateway.

Architecture:

```text
Payment Service
      |
      v
Payment Gateway Interface
      |
      +-- Dummy Gateway
      |
      +-- Future Gateway A
      |
      +-- Future Gateway B
```

The core order system should not directly depend on a specific payment provider.

---

# 17. Coupons Module

The Coupons module manages discount functionality.

Responsibilities include:

- Coupon creation
- Coupon validation
- Expiration
- Usage limits
- Minimum order requirements
- Discount calculation

Example:

```text
Order
  |
  +-- Subtotal
  |
  +-- Coupon
  |
  +-- Discount
  |
  +-- Final Total
```

---

# 18. Reviews Module

The Reviews module manages customer reviews and ratings.

Responsibilities include:

- Create review
- Update review
- Delete review
- Product rating
- Review listing
- Review validation

The system should ensure that review rules are enforced through the business layer.

---

# 19. Wishlist Module

The Wishlist module manages products saved by customers.

Responsibilities include:

- Add product
- Remove product
- View wishlist
- Check wishlist status

---

# 20. Inventory Module

The Inventory module manages product stock.

Responsibilities include:

- Stock quantity
- Stock updates
- Stock reservation
- Stock availability
- Low-stock detection

Inventory is a critical module because multiple customers may attempt to purchase the same product simultaneously.

Example:

```text
Product Stock = 10

Customer A -> Buy 4
Customer B -> Buy 3

Remaining Stock = 3
```

Inventory updates must maintain consistency.

---

# 21. Shipping Module

The Shipping module manages delivery-related information.

Responsibilities include:

- Shipping address
- Shipping status
- Shipment creation
- Tracking information
- Delivery status

Example:

```text
Order
  |
  v
Shipment
  |
  +-- Processing
  +-- Shipped
  +-- In Transit
  +-- Out for Delivery
  +-- Delivered
```

---

# 22. Notifications Module

The Notifications module manages communication with customers and sellers.

Potential notification types:

- Email
- In-app notification
- Order updates
- Payment updates
- Shipping updates
- Seller notifications

Architecture:

```text
Business Event
      |
      v
Notification Service
      |
      +-- Email
      +-- In-App
      +-- Future Channels
```

---

# 23. Chatbot Architecture

Vendora will contain two AI assistants:

```text
chatbots/
├── support_bot/
└── product_assist_bot/
```

---

# 24. Support Bot

The Support Bot will assist customers with support-related questions.

Potential capabilities include:

- Order status
- Basic order information
- Shipping information
- Return-related questions
- General marketplace support

Architecture:

```text
Customer
   |
   v
Support Bot
   |
   v
AI Provider
   |
   +-- Controlled Application APIs
          |
          +-- Orders
          +-- Shipping
          +-- Users
```

The AI should not have unrestricted direct database access.

---

# 25. Product Assistant Bot

The Product Assistant will help customers with product-related questions.

Potential capabilities include:

- Product information
- Product comparison
- Product discovery
- Product recommendations
- Product-related questions

Architecture:

```text
Customer
   |
   v
Product Assistant
   |
   v
AI Provider
   |
   v
Product / Catalog Data
```

Product information should be grounded in trusted application data.

---

# 26. AI Provider Abstraction

The AI architecture should avoid tightly coupling the application to one provider.

Example:

```text
AI Service
    |
    v
LLM Provider Interface
    |
    +-- Provider A
    |
    +-- Provider B
    |
    +-- Future Provider
```

This allows the AI provider to be changed without rewriting the chatbot business logic.

---

# 27. AI Failure Isolation

AI functionality must not become a dependency for core e-commerce functionality.

Example:

```text
                 AI Provider
                     |
                  FAILURE
                     |
                     v
             +---------------+
             | AI Assistant  |
             | Unavailable   |
             +---------------+

Core Marketplace:

Products     -> Works
Cart         -> Works
Checkout     -> Works
Orders       -> Works
Payments     -> Works
Shipping     -> Works
```

---

# 28. Database Architecture

The initial system will use a relational database.

Recommended initial database:

**PostgreSQL**

The database will contain data for the marketplace modules.

Example:

```text
PostgreSQL
    |
    +-- users
    +-- roles
    +-- addresses
    +-- sellers
    +-- products
    +-- categories
    +-- carts
    +-- cart_items
    +-- orders
    +-- order_items
    +-- payments
    +-- coupons
    +-- reviews
    +-- wishlists
    +-- inventory
    +-- shipments
    +-- notifications
```

---

# 29. Database Design Principles

The database design should prioritize:

- Data integrity
- Foreign-key relationships
- Appropriate indexes
- Transaction safety
- Normalization where appropriate
- Efficient queries
- Consistent monetary calculations

---

# 30. Database Migration

Database schema changes will be managed using:

**Alembic**

Example:

```text
Application
     |
     v
Alembic
     |
     v
PostgreSQL
```

Database schema changes must be version-controlled.

---

# 31. Authentication Architecture

Authentication will be based on token-based authentication.

High-level flow:

```text
User
 |
 | Login
 v
Auth API
 |
 v
Verify Credentials
 |
 v
Generate Token
 |
 v
Client
 |
 | Token
 v
Protected API
 |
 v
Authentication
 |
 v
Authorization
```

---

# 32. Authorization Architecture

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

Example:

```text
Customer
   |
   +-- Browse products
   +-- Manage cart
   +-- Place orders
   +-- Write reviews

Seller
   |
   +-- Manage own products
   +-- View seller orders
   +-- Manage inventory

Admin
   |
   +-- Manage users
   +-- Manage sellers
   +-- Manage products
   +-- Manage marketplace
```

---

# 33. Multi-Vendor Architecture

Vendora is a marketplace rather than a single-store e-commerce application.

A product must therefore be associated with a seller.

Example:

```text
Marketplace
 |
 +-- Seller A
 |     |
 |     +-- Product 1
 |     +-- Product 2
 |
 +-- Seller B
 |     |
 |     +-- Product 3
 |     +-- Product 4
 |
 +-- Seller C
       |
       +-- Product 5
```

Seller ownership must be enforced throughout the application.

---

# 34. Multi-Vendor Order Architecture

An order may contain products from multiple sellers.

Example:

```text
Customer
   |
   v
Order #1001
   |
   +-- Seller A
   |     |
   |     +-- Product 1
   |     +-- Product 2
   |
   +-- Seller B
         |
         +-- Product 3
```

The architecture should support seller-level order processing while maintaining the customer's overall order.

---

# 35. Order and Payment Flow

The initial checkout flow:

```text
Customer
    |
    v
Cart
    |
    v
Validate Cart
    |
    v
Validate Inventory
    |
    v
Calculate Total
    |
    v
Apply Coupon
    |
    v
Create Order
    |
    v
Payment
    |
    v
Payment Result
    |
    +-------- Failed
    |            |
    |            v
    |       Payment Failed
    |
    +-------- Successful
                 |
                 v
           Confirm Order
                 |
                 v
          Update Inventory
                 |
                 v
         Create Shipment
                 |
                 v
          Send Notification
```

The exact transaction and failure-handling strategy will be defined during implementation.

---

# 36. API Gateway Layer

The initial FastAPI application acts as the main API entry point.

Example:

```text
Client
   |
   v
FastAPI
   |
   +-- /api/v1/auth
   +-- /api/v1/users
   +-- /api/v1/products
   +-- /api/v1/categories
   +-- /api/v1/cart
   +-- /api/v1/orders
   +-- /api/v1/payments
   +-- /api/v1/coupons
   +-- /api/v1/reviews
   +-- /api/v1/wishlist
   +-- /api/v1/inventory
   +-- /api/v1/shipping
   +-- /api/v1/notifications
   +-- /api/v1/chatbots
```

---

# 37. Module Communication

Modules should communicate through clearly defined service interfaces.

Preferred:

```text
Orders
   |
   v
Inventory Service
```

Avoid:

```text
Orders
   |
   +---- Direct access to Inventory internals
   |
   +---- Direct access to Inventory database implementation
```

Business modules should not depend unnecessarily on the internal implementation details of other modules.

---

# 38. External Integration Architecture

External systems will be accessed through abstraction layers.

Example:

```text
                    Application
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
     Payment         Shipping       Notification
     Interface       Interface        Interface
          |              |              |
          v              v              v
     Provider A      Provider A      Provider A
```

This allows providers to be changed later without modifying the core business logic.

---

# 39. Caching Architecture

Caching may be introduced when performance requirements justify it.

Potential caching candidates include:

- Product catalog data
- Categories
- Frequently accessed product information
- Session-related data
- Rate limiting data

Potential technology:

**Redis**

Initial implementation should avoid unnecessary caching complexity.

Caching should be introduced based on measured performance requirements.

---

# 40. Background Job Architecture

Some operations should be moved to background processing as the system grows.

Potential background jobs:

- Email notifications
- Search indexing
- Image processing
- Report generation
- Notification delivery
- Non-critical AI processing

Potential architecture:

```text
Application
    |
    v
Message / Job Queue
    |
    +-- Worker 1
    +-- Worker 2
    +-- Worker 3
```

A queue technology such as Redis-based queues, RabbitMQ, or another appropriate system may be selected later.

---

# 41. Observability Architecture

The system should provide:

- Application logs
- Error logs
- Metrics
- Health checks
- Request tracing
- Business event monitoring

Example:

```text
Application
    |
    +-- Logs
    |
    +-- Metrics
    |
    +-- Health Checks
    |
    +-- Request IDs
    |
    v
Monitoring System
```

---

# 42. Error Handling Architecture

Errors should be handled consistently.

Example:

```text
Request
   |
   v
Router
   |
   v
Service
   |
   +---- Business Error
   |
   +---- Validation Error
   |
   +---- Database Error
   |
   +---- External Service Error
   |
   v
Exception Handler
   |
   v
Consistent API Error
```

Internal implementation details must not be exposed to clients.

---

# 43. Security Architecture

Security should be applied across multiple layers.

```text
Client
  |
  v
HTTPS
  |
  v
API
  |
  +-- Authentication
  |
  +-- Authorization
  |
  +-- Input Validation
  |
  +-- Rate Limiting
  |
  +-- Business Validation
  |
  v
Database
```

Sensitive information must be protected throughout the system.

---

# 44. File and Image Storage

Product images and other media should be separated from the core application storage where appropriate.

Initial architecture:

```text
React
  |
  v
FastAPI
  |
  v
Media Storage
```

The system should allow future integration with object storage.

Possible future storage systems include:

- S3-compatible storage
- Cloud object storage
- Self-hosted object storage

The specific provider will be selected later.

---

# 45. Deployment Architecture

Initial deployment may follow:

```text
                   Internet
                       |
                       v
                 Reverse Proxy
                       |
                       v
                FastAPI Backend
                       |
             +---------+---------+
             |                   |
             v                   v
        PostgreSQL             Redis
```

Frontend:

```text
Browser
   |
   v
React Application
   |
   v
FastAPI API
```

---

# 46. Development Environment

Developers should be able to run the main application locally.

Expected local components:

```text
Local Machine
 |
 +-- React
 |
 +-- FastAPI
 |
 +-- PostgreSQL
 |
 +-- Redis (when required)
```

---

# 47. Testing Architecture

Testing should exist at multiple levels.

```text
Tests
 |
 +-- Unit Tests
 |
 +-- Integration Tests
 |
 +-- API Tests
 |
 +-- End-to-End Tests
```

Critical business flows should receive stronger test coverage.

---

# 48. CI/CD Architecture

The repository should eventually use CI/CD automation.

Example:

```text
Developer
    |
    v
Git Push
    |
    v
Pull Request
    |
    v
CI Pipeline
    |
    +-- Lint
    +-- Type Checks
    +-- Unit Tests
    +-- Integration Tests
    +-- Build
    |
    v
Review
    |
    v
Merge
    |
    v
Deployment
```

The exact CI/CD platform and deployment environment will be decided later.

---

# 49. Git Branching Strategy

Development should not happen directly on `main`.

Recommended workflow:

```text
main
  |
  +-----------------------------+
                                |
                              feature
                                |
                                v
                           Development
                                |
                                v
                              Commit
                                |
                                v
                              Push
                                |
                                v
                          Pull Request
                                |
                                v
                              Review
                                |
                                v
                         Merge into main
```

For the current project workflow:

```text
main
  |
  +-- suri
        |
        +-- Documentation changes
        +-- Feature changes
        +-- Bug fixes
```

Pull Requests should be used to merge changes into `main`.

---

# 50. Future Microservice Architecture

If Vendora grows significantly, selected modules may become independent services.

Potential future architecture:

```text
                         API Gateway
                              |
       +----------+-----------+-----------+----------+
       |          |           |           |          |
       v          v           v           v          v
   Identity    Catalog      Cart        Orders    Payments
   Service     Service     Service      Service    Service
                              |
                              v
                         Inventory
                          Service
                              |
                    +---------+---------+
                    |                   |
                    v                   v
                Shipping          Notifications
                 Service              Service

                              |
                              v
                         AI Gateway
                              |
                    +---------+---------+
                    |                   |
                    v                   v
               Support Bot       Product Bot
```

Microservices should only be introduced when there is a real operational or scaling reason.

---

# 51. Potential Future Service Boundaries

The current modules can potentially become:

```text
Identity Service
    |
    +-- Users
    +-- Authentication
    +-- Authorization

Catalog Service
    |
    +-- Products
    +-- Categories

Cart Service
    |
    +-- Cart
    +-- Wishlist

Order Service
    |
    +-- Orders
    +-- Order Items

Payment Service
    |
    +-- Payments

Inventory Service
    |
    +-- Inventory

Shipping Service
    |
    +-- Shipping

Notification Service
    |
    +-- Notifications

AI Service
    |
    +-- Support Bot
    +-- Product Assistant
```

These are future architectural possibilities, not requirements for the initial implementation.

---

# 52. Architectural Principles

Vendora development should follow these principles:

### 52.1 Separation of Concerns

Each module should have a clear responsibility.

### 52.2 Single Responsibility

A component should have a focused responsibility.

### 52.3 Loose Coupling

Modules should avoid unnecessary dependencies.

### 52.4 High Cohesion

Related functionality should remain together.

### 52.5 Security by Design

Security should be considered during design rather than added at the end.

### 52.6 API First

Backend functionality should be exposed through clearly defined APIs.

### 52.7 Provider Independence

External services should be abstracted where practical.

### 52.8 Testability

Business logic should be designed so it can be tested independently.

### 52.9 Observability

Important system behavior should be measurable and traceable.

### 52.10 Simplicity First

The architecture should not introduce complexity without a clear requirement.

---

# 53. Initial Technology Stack

The initial technology stack is planned as follows:

| Layer | Technology |
|------|------------|
| Frontend | React |
| Backend | FastAPI |
| Language | Python |
| API | REST |
| Database | PostgreSQL |
| ORM / Database Access | SQLAlchemy |
| Database Migration | Alembic |
| Validation | Pydantic |
| Authentication | JWT-based authentication |
| State Management | Zustand or Redux Toolkit |
| Server State | TanStack Query |
| Cache / Future Queue | Redis |
| AI Integration | Provider abstraction |
| Testing | Pytest |
| Version Control | Git |
| Repository | GitHub |

Specific library versions will be defined during implementation.

---

# 54. Initial Project Structure

The backend repository is expected to follow this structure:

```text
vendora/
│
├── backend/
│   ├── app/
│   │   ├── core/
│   │   │   ├── config.py
│   │   │   ├── security.py
│   │   │   ├── database.py
│   │   │   └── dependencies.py
│   │   │
│   │   ├── modules/
│   │   │   ├── users/
│   │   │   ├── products/
│   │   │   ├── categories/
│   │   │   ├── cart/
│   │   │   ├── orders/
│   │   │   ├── payments/
│   │   │   ├── coupons/
│   │   │   ├── reviews/
│   │   │   ├── wishlist/
│   │   │   ├── inventory/
│   │   │   ├── shipping/
│   │   │   └── notifications/
│   │   │
│   │   ├── chatbots/
│   │   │   ├── support_bot/
│   │   │   └── product_assist_bot/
│   │   │
│   │   └── main.py
│   │
│   ├── alembic/
│   ├── tests/
│   ├── requirements.txt
│   └── README.md
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── README.md
│
├── docs/
│   ├── 01-project-overview.md
│   ├── 02-functional-requirements.md
│   ├── 03-non-functional-requirements.md
│   └── 04-system-architecture.md
│
└── README.md
```

---

# 55. Request Lifecycle

A typical API request should follow:

```text
Client
  |
  v
FastAPI Router
  |
  v
Request Validation
  |
  v
Authentication
  |
  v
Authorization
  |
  v
Service Layer
  |
  v
Repository Layer
  |
  v
Database
  |
  v
Repository
  |
  v
Service
  |
  v
Response Schema
  |
  v
Client
```

---

# 56. Example Product Creation Flow

A seller creating a product:

```text
Seller
  |
  v
POST /api/v1/products
  |
  v
Authentication
  |
  v
Seller Authorization
  |
  v
Validate Request
  |
  v
Product Service
  |
  v
Product Repository
  |
  v
PostgreSQL
  |
  v
Product Created
  |
  v
API Response
```

---

# 57. Example Order Flow

Customer checkout:

```text
Customer
  |
  v
Cart
  |
  v
Checkout API
  |
  v
Validate Cart
  |
  v
Validate Inventory
  |
  v
Calculate Price
  |
  v
Apply Coupon
  |
  v
Create Order
  |
  v
Payment
  |
  v
Confirm Payment
  |
  v
Update Order
  |
  v
Update Inventory
  |
  v
Create Shipment
  |
  v
Notification
```

---

# 58. Architecture Decision Summary

| Decision | Initial Choice |
|----------|----------------|
| Architecture | Modular Monolith |
| Backend | FastAPI |
| Frontend | React |
| Database | PostgreSQL |
| API Style | REST |
| Database Migration | Alembic |
| Validation | Pydantic |
| Authentication | JWT-based |
| Server State | TanStack Query |
| Client State | Zustand / Redux Toolkit |
| Cache | Redis when required |
| AI | Provider abstraction |
| Testing | Pytest |
| Repository | GitHub |
| Future Architecture | Selective Microservices |

---

# 59. Architecture Evolution

Vendora will evolve in stages.

## Phase 1 — Foundation

```text
React
   |
FastAPI Modular Monolith
   |
PostgreSQL
```

Focus:

- Project setup
- Authentication
- Users
- Products
- Categories
- Basic marketplace functionality

---

## Phase 2 — Marketplace Features

Add:

- Cart
- Orders
- Payments
- Coupons
- Reviews
- Wishlist
- Inventory
- Shipping
- Notifications

---

## Phase 3 — AI Features

Add:

- Support Bot
- Product Assistant
- RAG where required
- Tool/function calling where required
- AI observability

---

## Phase 4 — Scale and Optimization

Introduce where justified:

- Redis
- Background workers
- Message queues
- Search infrastructure
- Caching
- Performance optimization

---

## Phase 5 — Microservice Extraction

Only if required:

```text
Modular Monolith
       |
       v
Identify Bottleneck
       |
       v
Extract Module
       |
       v
Independent Service
```

Possible candidates:

- Orders
- Payments
- Inventory
- Notifications
- Catalog
- AI

---

# 60. Architecture Constraints

The initial system should follow these constraints:

1. Do not introduce microservices without a clear requirement.
2. Do not allow modules to directly depend on each other's internal implementation.
3. Do not put business logic inside API routers.
4. Do not tightly couple payment logic to a specific provider.
5. Do not give AI components unrestricted database access.
6. Do not store secrets in source code.
7. Do not bypass authorization checks.
8. Do not compromise inventory consistency for performance.
9. Do not make AI functionality a dependency for core marketplace operations.
10. Do not introduce infrastructure complexity before it is required.

---

# 61. Architecture Success Criteria

The architecture will be considered successful if it provides:

- Clear module boundaries
- Reliable order processing
- Consistent inventory management
- Secure authentication and authorization
- Replaceable external integrations
- Testable business logic
- Observable application behavior
- Reasonable API performance
- Ability to scale individual components when required
- Ability to extract selected modules into services later

---

# 62. Document Status

**Document:** System Architecture  
**Version:** 1.0  
**Status:** Initial Draft  
**Project:** Vendora  
**Repository:** `vendora-multi-vendor-ecommerce`

This document defines the initial architecture for Vendora.

Detailed technical decisions, database design, API contracts, deployment architecture, and individual module designs will be documented separately as the project progresses.