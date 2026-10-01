# 🔧 FixItNow Backend

<p align="center">
  <strong>Production-Ready REST API for a Home Services Marketplace</strong>
</p>

<p align="center">
  A scalable TypeScript backend powering customer bookings, technician management, role-based authorization, Stripe payments, reviews, service management, and administrative operations.
</p>

<p align="center">
  <a href="https://fixitnow-frontend-xi.vercel.app/">🌐 Live Application</a> •
  <a href="https://github.com/mmahadi-ahmedd">💻 GitHub Profile</a>
</p>

---

# 🚀 About The Project

**FixItNow Backend** is the REST API behind a full-stack home services marketplace.

The platform connects customers with professional technicians for services such as:

* Plumbing
* Electrical work
* Cleaning
* Painting
* Maintenance
* Other household services

The backend is responsible for authentication, authorization, service discovery, technician management, booking workflows, payment processing, reviews, and administrative operations.

The API was designed with a focus on **real-world backend engineering rather than basic CRUD**.

Key backend concerns include:

* RESTful API architecture
* Type-safe development with TypeScript
* PostgreSQL relational database
* Prisma ORM
* JWT authentication
* Role-based access control
* Server-side Zod validation
* Secure password hashing
* Booking state management
* Stripe Checkout
* Stripe webhooks
* Database transactions
* Structured error responses
* Protected resources
* Production deployment

---

# 🧩 Backend Architecture

```text
                         ┌──────────────────────┐
                         │      Next.js App     │
                         │       Frontend       │
                         └──────────┬───────────┘
                                    │
                              HTTP / REST
                                    │
                                    ▼
                    ┌─────────────────────────────┐
                    │        Express API          │
                    │                             │
                    │  Routes                     │
                    │      ↓                      │
                    │  Controllers                │
                    │      ↓                      │
                    │  Services / Business Logic  │
                    │      ↓                      │
                    │  Prisma ORM                 │
                    └─────────────┬───────────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │     PostgreSQL      │
                       └─────────────────────┘

                                  ▲
                                  │
                       ┌─────────────────────┐
                       │       Stripe        │
                       │ Checkout + Webhooks │
                       └─────────────────────┘
```

The application separates HTTP handling from business logic and database operations to keep the backend maintainable and easier to extend.

---

# 🛠️ Tech Stack

| Technology        | Purpose                                   |
| ----------------- | ----------------------------------------- |
| **Node.js**       | JavaScript runtime                        |
| **Express.js**    | REST API framework                        |
| **TypeScript**    | Static type safety                        |
| **Prisma ORM**    | Database access, relations & transactions |
| **PostgreSQL**    | Relational database                       |
| **JWT**           | Authentication                            |
| **bcryptjs**      | Password hashing                          |
| **Stripe**        | Payment processing                        |
| **Zod**           | Server-side validation                    |
| **cookie-parser** | Cookie handling                           |
| **CORS**          | Cross-origin configuration                |
| **tsup**          | Production bundling                       |
| **Vercel**        | Deployment                                |

---

# 👥 User Roles

FixItNow has three fixed roles.

```text
                    ┌─────────────┐
                    │    USER     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
     CUSTOMER         TECHNICIAN          ADMIN
```

## 👤 Customer

Customers can:

* Register and authenticate
* Browse services
* Search technicians
* View technician profiles
* Create bookings
* Track booking status
* Pay for accepted bookings
* View payment history
* Leave reviews
* Manage their profile

## 🛠️ Technician

Technicians can:

* Create their professional profile
* Add/update services
* Manage pricing
* Set availability
* View incoming bookings
* Accept bookings
* Decline bookings
* Start jobs
* Complete jobs

## 🛡️ Admin

Admins can:

* View all users
* Ban/unban users
* View all bookings
* Manage service categories
* Monitor platform activity

---

# 🔐 Authentication

The API uses **JWT-based authentication**.

Authentication flow:

```text
Registration
     ↓
Password Hashing
     ↓
User Created
     ↓
Login
     ↓
Credential Verification
     ↓
JWT Tokens
     ↓
Authenticated API Requests
```

### Security

Passwords are never stored as plain text.

```text
Plain Password
      ↓
bcrypt
      ↓
Password Hash
      ↓
PostgreSQL
```

Protected endpoints require authenticated users, while role-specific endpoints additionally verify authorization.

---

# 🛡️ Role-Based Access Control

Authentication answers:

> "Who is this user?"

Authorization answers:

> "What is this user allowed to do?"

FixItNow separates these responsibilities.

Example:

```text
Authenticated User
       │
       ▼
Authentication Middleware
       │
       ▼
Current User
       │
       ▼
Role Authorization
       │
 ┌─────┼─────────────┐
 ▼     ▼             ▼
User  Technician    Admin
```

For example:

* Customers can create bookings.
* Technicians can manage their own bookings.
* Admins can manage platform users and categories.

---

# 📡 REST API

The API follows REST-oriented resource design.

## Authentication

| Method | Endpoint             | Purpose           |
| ------ | -------------------- | ----------------- |
| `POST` | `/api/auth/register` | Register user     |
| `POST` | `/api/auth/login`    | Authenticate user |
| `GET`  | `/api/auth/me`       | Get current user  |

---

## Services & Technicians

| Method | Endpoint               | Purpose            |
| ------ | ---------------------- | ------------------ |
| `GET`  | `/api/services`        | Browse services    |
| `GET`  | `/api/technicians`     | Browse technicians |
| `GET`  | `/api/technicians/:id` | Technician details |
| `GET`  | `/api/categories`      | Service categories |

---

## Bookings

| Method | Endpoint            | Purpose             |
| ------ | ------------------- | ------------------- |
| `POST` | `/api/bookings`     | Create booking      |
| `GET`  | `/api/bookings`     | Get user's bookings |
| `GET`  | `/api/bookings/:id` | Get booking details |

---

## Payments

| Method | Endpoint                | Purpose                |
| ------ | ----------------------- | ---------------------- |
| `POST` | `/api/payments/create`  | Create payment session |
| `POST` | `/api/payments/confirm` | Confirm/verify payment |
| `GET`  | `/api/payments`         | Payment history        |
| `GET`  | `/api/payments/:id`     | Payment details        |

---

## Technician Management

| Method  | Endpoint                       | Purpose                 |
| ------- | ------------------------------ | ----------------------- |
| `PUT`   | `/api/technician/profile`      | Update profile          |
| `PUT`   | `/api/technician/availability` | Update availability     |
| `GET`   | `/api/technician/bookings`     | Get technician bookings |
| `PATCH` | `/api/technician/bookings/:id` | Update booking status   |

---

## Reviews

| Method | Endpoint       | Purpose       |
| ------ | -------------- | ------------- |
| `POST` | `/api/reviews` | Create review |

---

## Admin

| Method  | Endpoint                | Purpose           |
| ------- | ----------------------- | ----------------- |
| `GET`   | `/api/admin/users`      | List users        |
| `PATCH` | `/api/admin/users/:id`  | Ban/unban user    |
| `GET`   | `/api/admin/bookings`   | List all bookings |
| `GET`   | `/api/admin/categories` | List categories   |
| `POST`  | `/api/admin/categories` | Create category   |

---

# 🧠 Booking State Machine

One of the most important pieces of business logic is the booking lifecycle.

```text
                       ┌─────────────┐
                       │  REQUESTED  │
                       └──────┬──────┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
                Accept              Decline
                    │                   │
                    ▼                   ▼
              ┌──────────┐       ┌──────────┐
              │ ACCEPTED │       │ DECLINED │
              └────┬─────┘       └──────────┘
                   │
                   ▼
               ┌───────┐
               │ PAID  │
               └───┬───┘
                   │
                   ▼
            ┌─────────────┐
            │ IN_PROGRESS │
            └──────┬──────┘
                   │
                   ▼
             ┌───────────┐
             │ COMPLETED │
             └───────────┘
```

The backend does not allow arbitrary status changes.

For example:

```text
REQUESTED → ACCEPTED       ✅
ACCEPTED  → PAID           ✅
PAID      → IN_PROGRESS    ✅
IN_PROGRESS → COMPLETED    ✅

REQUESTED → COMPLETED      ❌
ACCEPTED  → COMPLETED      ❌
DECLINED  → PAID           ❌
```

This protects the integrity of the booking workflow even if someone bypasses the frontend.

---

# 💳 Stripe Payment Integration

Payment processing uses **real Stripe integration**.

There is no simulated "Pay Now" implementation.

### Payment Architecture

```text
Customer
   │
   ▼
Accepted Booking
   │
   ▼
POST /api/payments/create
   │
   ▼
Stripe Checkout Session
   │
   ▼
Stripe Hosted Checkout
   │
   ▼
Successful Payment
   │
   ▼
Stripe Webhook
   │
   ▼
Backend Verification
   │
   ▼
Database Transaction
   │
   ├───────────────┐
   ▼               ▼
Payment         Booking
COMPLETED         PAID
```

The frontend does not decide whether a payment was successful.

The backend receives and verifies the Stripe webhook and updates the relevant database records.

---

# 🔔 Stripe Webhooks

Webhooks allow Stripe to communicate payment events directly with the backend.

```text
Stripe
  │
  │ payment event
  ▼
Webhook Endpoint
  │
  ▼
Verify Stripe Signature
  │
  ▼
Identify Payment
  │
  ▼
Update Database
  │
  ├── Payment Status
  └── Booking Status
```

This prevents the frontend from being the source of truth for payment completion.

---

# 💾 Database Design

The backend uses **PostgreSQL with Prisma ORM**.

Core entities:

```text
┌─────────────┐
│    User     │
└──────┬──────┘
       │
       ├───────────────┐
       │               │
       ▼               ▼
┌───────────────┐  ┌─────────────┐
│Technician     │  │  Bookings   │
│Profile        │  └──────┬──────┘
└───────┬───────┘         │
        │                 ├──────────────┐
        ▼                 ▼              ▼
┌───────────────┐   ┌──────────┐   ┌──────────┐
│   Services    │   │ Payments │   │ Reviews  │
└───────┬───────┘   └──────────┘   └──────────┘
        │
        ▼
┌───────────────┐
│  Categories   │
└───────────────┘
```

### Main Models

* `User`
* `TechnicianProfile`
* `Category`
* `Service`
* `Booking`
* `Payment`
* `Review`

Prisma manages:

* Relations
* Migrations
* Transactions
* Queries
* Type-safe database access
* Seed data

---

# 🔗 Relational Data Model

The database is designed around relational ownership.

Examples:

```text
User
 └── TechnicianProfile
       └── Services
             └── Category
```

and:

```text
Customer
   │
   └── Booking
          ├── Technician
          ├── Service
          └── Payment
```

Reviews connect customers with completed technician jobs.

This avoids duplicating data unnecessarily and keeps relationships explicit.

---

# 🔄 Database Transactions

Critical multi-step operations use database transactions where consistency matters.

For example, payment processing may require related state changes:

```text
BEGIN TRANSACTION

    Update Payment
        ↓
    Update Booking

COMMIT
```

If an operation fails:

```text
BEGIN TRANSACTION

    Update Payment
        ↓
       ERROR

ROLLBACK
```

This prevents partially updated business state.

---

# ✅ Server-Side Validation

Every important API endpoint validates incoming data on the server.

**Zod** is used for schema validation.

```text
Client Request
      ↓
Zod Schema
      ↓
Valid?
  /       \
YES       NO
 │         │
 ▼         ▼
Service   400 Error
Layer
```

Example invalid request:

```json
{
  "price": "not-a-number"
}
```

The server rejects the request instead of trusting the frontend.

---

# 🧱 Consistent Error Responses

The API follows a predictable error structure.

```json
{
  "success": false,
  "message": "Booking cannot be accepted in its current state.",
  "errorDetails": {}
}
```

This provides a consistent contract between frontend and backend.

Common HTTP responses include:

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
500 Internal Server Error
```

---

# 🧰 Middleware Architecture

The backend uses middleware to centralize cross-cutting concerns.

Typical request flow:

```text
Incoming Request
       ↓
CORS
       ↓
Cookie Parser
       ↓
Authentication
       ↓
Authorization
       ↓
Validation
       ↓
Route
       ↓
Controller
       ↓
Service
       ↓
Prisma
       ↓
PostgreSQL
```

This prevents authentication, validation, and authorization logic from being duplicated throughout controllers.

---

# 🏗️ Backend Structure

A typical backend organization:

```text
src/
├── app/
│   ├── routes/
│   ├── controllers/
│   ├── services/
│   ├── middlewares/
│   ├── validations/
│   └── utils/
│
├── config/
│
├── modules/
│   ├── auth/
│   ├── users/
│   ├── services/
│   ├── technicians/
│   ├── bookings/
│   ├── payments/
│   ├── reviews/
│   └── admin/
│
├── lib/
│   ├── prisma/
│   └── stripe/
│
├── types/
│
└── server.ts

prisma/
├── schema.prisma
├── migrations/
└── seed.ts
```

The exact folder structure may evolve with the implementation, but the architecture separates HTTP concerns, business logic, validation, and persistence.

---

# 🔐 Authentication Flow

```text
POST /api/auth/register
          │
          ▼
     Validate Input
          │
          ▼
    Hash Password
          │
          ▼
      Create User
```

Login:

```text
POST /api/auth/login
          │
          ▼
     Validate Input
          │
          ▼
   Find User
          │
          ▼
Compare Password
          │
          ▼
 Generate JWT
          │
          ▼
Authenticated Session
```

Protected request:

```text
Client
  │
  ▼
JWT / Cookie
  │
  ▼
Auth Middleware
  │
  ▼
Verify Token
  │
  ▼
Identify User
  │
  ▼
Authorization
  │
  ▼
Controller
```

---

# 👤 User Status Management

Admins can manage account status.

```text
ACTIVE
  │
  │ Admin bans user
  ▼
BANNED
  │
  │ Admin restores user
  ▼
ACTIVE
```

Account status is checked by protected backend operations rather than being purely a frontend UI restriction.

---

# 📊 Service & Technician Discovery

Public endpoints support service discovery.

Customers can:

* Browse services
* Search services
* Filter by category
* Discover technicians
* View technician details
* Review pricing
* View ratings
* View reviews

Example:

```text
GET /api/services
GET /api/technicians
GET /api/technicians/:id
GET /api/categories
```

---

# 📅 Availability Management

Technicians can manage their available time slots.

```text
Technician
    ↓
Update Availability
    ↓
Backend Validation
    ↓
Persist Availability
    ↓
Customer Can View Available Slots
```

This availability data becomes part of the booking workflow.

---

# ⭐ Reviews

Customers can leave reviews after completing a job.

The backend verifies the business rules before creating the review.

```text
Customer
   ↓
Completed Booking?
   ↓
   YES
   ↓
Create Review
   ↓
Store Rating + Comment
```

This prevents arbitrary users from reviewing technicians without a valid completed service relationship.

---

# 🌍 Environment Configuration

Environment-specific values are kept outside the source code.

Typical configuration includes:

```env
DATABASE_URL=

JWT_ACCESS_SECRET=
JWT_REFRESH_SECRET=

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=

CLIENT_URL=

NODE_ENV=
```

Secrets are not committed to GitHub.

---

# 🚀 Local Development

## 1. Clone the repository

```bash
git clone YOUR_BACKEND_REPOSITORY_URL
```

## 2. Enter the project

```bash
cd fixitnow-backend
```

## 3. Install dependencies

```bash
npm install
```

## 4. Configure environment variables

Create:

```text
.env
```

and configure PostgreSQL, JWT, Stripe, and application settings.

## 5. Generate Prisma Client

```bash
npx prisma generate
```

## 6. Run migrations

```bash
npx prisma migrate dev
```

## 7. Seed the database

```bash
npm run seed
```

## 8. Start development server

```bash
npm run dev
```

---

# 📦 Useful Commands

```bash
npm run dev
```

Start development server.

```bash
npm run build
```

Build the backend for production.

```bash
npm run start
```

Start the production server.

```bash
npx prisma generate
```

Generate Prisma Client.

```bash
npx prisma migrate dev
```

Create/apply development migrations.

```bash
npx prisma studio
```

Open Prisma Studio.

---

# 🌐 Deployment

The backend is designed for cloud deployment using Vercel serverless functions.

The production architecture connects:

```text
Next.js Frontend
       │
       ▼
Vercel
       │
       ▼
Express Backend
       │
       ├───────────────┐
       ▼               ▼
 PostgreSQL          Stripe
```

Environment-specific configuration keeps development and production settings separate.

---

# 📚 API Documentation

The backend assignment requires complete API documentation covering all endpoints.

The documentation should include:

* Authentication endpoints
* Services
* Technicians
* Categories
* Bookings
* Payments
* Reviews
* Technician management
* Admin operations
* Request examples
* Response examples
* Authentication requirements
* Validation/error examples

**API Documentation:** Add the published Postman/Swagger URL here once available.

---

# 🧪 API Testing

The backend can be tested using:

* Postman
* Thunder Client
* REST Client
* Frontend integration

A complete demonstration covers:

```text
Customer
   ↓
Register/Login
   ↓
Browse Services
   ↓
Create Booking
   ↓
Technician
   ↓
Accept Booking
   ↓
Customer
   ↓
Stripe Payment
   ↓
Webhook
   ↓
Booking → PAID
```

---

# 📋 Required API Error Scenarios

The backend handles cases such as:

### Unauthorized request

```text
401 Unauthorized
```

### Insufficient permissions

```text
403 Forbidden
```

### Resource not found

```text
404 Not Found
```

### Invalid booking transition

```text
409 Conflict
```

### Invalid input

```text
400 Bad Request
```

This makes API behavior predictable for frontend consumers.

---

# 🧠 Important Engineering Decisions

## 1. Business Logic Lives on the Backend

The frontend can hide or disable buttons, but the backend remains the final authority.

For example:

```text
Frontend:
"Accept Booking" button hidden

BUT

Backend:
PATCH /api/technician/bookings/:id
       ↓
Verify technician
       ↓
Verify booking ownership
       ↓
Verify current status
       ↓
Allow transition
```

This prevents clients from bypassing business rules.

---

## 2. Payment Is Backend-Controlled

The frontend never directly changes:

```text
Payment = COMPLETED
```

Instead:

```text
Stripe
   ↓
Webhook
   ↓
Backend
   ↓
Database
```

---

## 3. Validation Happens Twice

```text
Frontend
   ↓
Better UX

Backend
   ↓
Actual Data Integrity
```

Server-side validation ensures the API remains safe even when called directly through Postman or another client.

---

## 4. Database Relations Are Explicit

Prisma relations make ownership and dependencies clear:

```text
User
 ↓
Booking
 ↓
Service
 ↓
Technician
```

This makes complex queries and transactional operations easier to reason about.

---

# 📝 Git & Development History

The backend was developed incrementally with meaningful commits rather than a single final upload.

Examples:

```text
feat: implement authentication module
feat: add role based authorization
feat: create service management endpoints
feat: implement technician profile management
feat: add booking creation workflow
feat: implement technician booking actions
feat: add Stripe checkout integration
feat: implement Stripe webhook handler
feat: add payment history endpoints
feat: implement review system
feat: add admin user management
feat: add category management
fix: resolve booking state transition
fix: improve validation error response
fix: handle payment webhook synchronization
refactor: centralize error handling
refactor: improve service layer structure
```

The assignment requirement is **20 meaningful backend commits**, and the repository should preserve that development history.

---

# 📈 Backend Highlights

| Area                 | Implementation      |
| -------------------- | ------------------- |
| Runtime              | Node.js             |
| Framework            | Express.js          |
| Language             | TypeScript          |
| Database             | PostgreSQL          |
| ORM                  | Prisma              |
| Authentication       | JWT                 |
| Password Security    | bcryptjs            |
| Authorization        | RBAC                |
| Validation           | Zod                 |
| Payments             | Stripe              |
| Payment Events       | Webhooks            |
| Database Consistency | Prisma Transactions |
| API Style            | REST                |
| Error Handling       | Structured JSON     |
| Cookies              | cookie-parser       |
| CORS                 | Express CORS        |
| Bundling             | tsup                |
| Deployment           | Vercel              |

---

# 🎯 What This Backend Demonstrates

FixItNow is designed to demonstrate practical backend engineering skills:

### API Design

RESTful resource-oriented endpoints with predictable HTTP semantics.

### Authentication

JWT-based authentication with protected API routes.

### Authorization

Role-based access control for customer, technician, and admin workflows.

### Database Engineering

PostgreSQL relational modeling through Prisma.

### Business Logic

Booking lifecycle and state-transition enforcement.

### Payment Engineering

Stripe Checkout, payment verification, and webhook-driven state synchronization.

### Validation

Zod-based server-side request validation.

### Error Handling

Consistent structured API error responses.

### Deployment

Production-oriented environment configuration and cloud deployment.

---

# 🔮 Future Improvements

Potential improvements include:

* Automated integration tests
* Unit tests for service-layer business logic
* Rate limiting
* API request logging
* Advanced booking conflict detection
* Email notifications
* SMS notifications
* Real-time booking updates
* Refund handling
* Stripe payment retry handling
* Technician verification
* Advanced analytics
* Background job processing
* API versioning
* OpenAPI/Swagger automation

---

# 👨‍💻 Developer

## Mahadi Mahbub Ahmed

**Frontend Developer | Full-Stack Developer**

I build practical web applications with modern frontend technologies and backend systems focused on clean architecture, reliable APIs, authentication, database design, and real-world business workflows.

<p align="center">
  <a href="https://github.com/mmahadi-ahmedd">💻 GitHub</a> •
  <a href="https://www.linkedin.com/in/mahadi-ahmed/">💼 LinkedIn</a> •
  <a href="https://mahadi-ahmed-portfolio-67a1e.web.app/">🌐 Portfolio</a> •
  <a href="mailto:ahmedmahadi2003@gmail.com">📧 Email</a>
</p>

---

# 🔗 Project Links

| Resource            | Link                                                          |
| ------------------- | ------------------------------------------------------------- |
| 🌐 Live Application | https://fixitnow-frontend-xi.vercel.app/                      |
| 💻 GitHub Profile   | https://github.com/mmahadi-ahmedd                             |
| 💼 LinkedIn         | https://www.linkedin.com/in/mahadi-ahmed/                     |
| 🌐 Portfolio        | https://mahadi-ahmed-portfolio-67a1e.web.app/                 |
| 📧 Email            | [ahmedmahadi2003@gmail.com](mailto:ahmedmahadi2003@gmail.com) |

---

# ⭐ FixItNow Backend

A backend built around real application requirements:

```text
TypeScript
    +
Express
    +
Prisma
    +
PostgreSQL
    +
JWT
    +
RBAC
    +
Zod
    +
Stripe
    +
Webhooks
    +
Transactions
    +
REST API
```

**FixItNow — Your Trusted Home Service Platform.**

<p align="center">
  Built with TypeScript, Express, Prisma, PostgreSQL & Stripe.
</p>
