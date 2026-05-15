# AuthPlatform

Enterprise-grade Authentication & Authorization Platform built with **ASP.NET Core**, **Clean Architecture**, **JWT Authentication**, **Role & Permission Authorization**, **EF Core**, **Dapper**, and **RDLC Reporting**.

---

# 🚀 Overview

AuthPlatform is a scalable enterprise-ready authentication and authorization solution designed using **Clean Architecture** principles.

It provides:

- Custom Authentication
- JWT + Refresh Token security
- Role-based authorization
- User-level permission overrides
- Audit logging
- Login history tracking
- Session-based JWT handling in MVC
- RDLC reporting with PDF/Excel export
- EF Core + Dapper hybrid data access

---

# 🏗 Architecture

The solution follows **Clean Architecture** with strict dependency flow:

```text
┌─────────────────────────────┐
│        AuthPlatform.Mvc     │
│  (UI Client / RDLC Reports) │
└──────────────┬──────────────┘
               │ HTTP API Calls
               ▼
┌─────────────────────────────┐
│        AuthPlatform.Api     │
│ (REST API + Middleware)     │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│   AuthPlatform.Application  │
│ (Use Cases / Services / DTOs)
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│     AuthPlatform.Domain     │
│ (Entities / Interfaces)     │
└──────────────┬──────────────┘
               ▲
               │
┌─────────────────────────────┐
│ AuthPlatform.Infrastructure │
│ EF Core / Dapper / JWT      │
└─────────────────────────────┘
```

---

# 📁 Solution Structure

```text
AuthPlatform.sln
│
├── AuthPlatform.Domain
├── AuthPlatform.Application
├── AuthPlatform.Infrastructure
├── AuthPlatform.Api
└── AuthPlatform.Mvc
```

---

# 📦 Project Responsibilities

---

## 1. AuthPlatform.Domain

Contains:
- Entities
- Repository interfaces
- Business rules
- Shared base models

### Example

```text
Entities/
Interfaces/
Common/
```

---

## 2. AuthPlatform.Application

Contains:
- Use cases
- Services
- DTOs
- Validators
- Mapping profiles

### Example

```text
DTOs/
Services/
Validators/
Mappings/
```

---

## 3. AuthPlatform.Infrastructure

Contains:
- EF Core DbContext
- Dapper repositories
- JWT generation
- Password hashing
- SQL scripts
- Repository implementations

### Example

```text
Persistence/
Repositories/
Security/
SQL/
```

---

## 4. AuthPlatform.Api

Contains:
- REST APIs
- Middleware
- Filters
- Authentication pipeline

### Example

```text
Controllers/
Middleware/
Filters/
```

---

## 5. AuthPlatform.Mvc

Contains:
- MVC UI
- Session-based token management
- RDLC reporting
- API communication layer

### Example

```text
Controllers/
Views/
Reports/
Services/
Session/
```

---

# 🔐 Authentication & Authorization Design

---

## Authentication Flow

```text
User Login
    │
    ▼
MVC sends credentials to API
    │
    ▼
API validates user
    │
    ▼
JWT + Refresh Token generated
    │
    ▼
MVC stores tokens in Server Session
    │
    ▼
Browser stores ONLY Secure HttpOnly Cookie
```

---

## Authorization Model

Supports:

### Role-Based Permissions

```text
Admin
 ├── User.Create
 ├── User.Edit
 └── Report.View
```

### User-Based Permissions

```text
John Doe
 ├── Report.Export
 └── User.Delete
```

### Effective Permission Resolution

```text
Final Permissions =
Role Permissions
+ User Permissions
```

---

# 🧱 Database Design

---

## Core Authentication Tables

| Table | Purpose |
|---|---|
| AuthUsers | System users |
| AuthRoles | User roles |
| AuthPermissions | Available permissions |
| AuthUserPermissions | User-level permissions |
| AuthRolePermissions | Role-level permissions |
| AuthLoginHistories | Login tracking |
| AuthAuditActivities | Audit logging |
| RefreshTokens | JWT refresh tokens |

---

# ⚙️ Technology Stack

---

## Backend

- ASP.NET Core Web API
- ASP.NET Core MVC
- Clean Architecture
- Entity Framework Core
- Dapper
- SQL Server

---

## Security

- JWT Authentication
- Refresh Tokens
- PBKDF2 / BCrypt Password Hashing
- Claims-based Authorization
- Permission-based Authorization

---

## Reporting

- RDLC Reports
- PDF Export
- Excel Export

---

## Validation & Mapping

- FluentValidation
- AutoMapper

---

## Logging & Auditing

- Audit Activity Logging
- Login History Tracking

---

# 🧩 Data Access Strategy

---

## EF Core

Used for:
- Create
- Update
- Delete
- Migrations
- Entity tracking

---

## Dapper

Used for:
- Reports
- Read-heavy queries
- Dashboard summaries
- Permission queries
- Stored procedures
- Fast lookups

---

# 🔒 Security Design

---

## API Security

- Stateless API
- JWT Bearer Authentication
- Permission middleware
- Claims validation

---

## MVC Security

- No JWT in localStorage/sessionStorage
- Tokens stored in server-side session only
- Browser only receives Secure HttpOnly cookie

---

## Password Security

Supports:
- PBKDF2
- BCrypt

---

# 📊 RDLC Reporting

---

## Supported Reports

### Without Parameters

- Active Users Report
- Dashboard Summary Report

### With Parameters

- User Permission Report
- Role Permission Report
- Login History Report
- Audit Activity Report

---

## Export Formats

- PDF
- Excel

---

# 🔄 Request Flow

```text
Browser
   │
   ▼
MVC Application
   │
   ▼
API Gateway
   │
   ▼
Application Services
   │
   ▼
Infrastructure Layer
   │
   ▼
SQL Server
```

---

# 🧠 Clean Architecture Rules

---

## Dependency Rule

```text
Outer Layers depend on Inner Layers
Inner Layers NEVER depend on Outer Layers
```

---

## Rules Applied

✅ MVC NEVER accesses database  
✅ API remains stateless  
✅ No classic UnitOfWork pattern  
✅ Repository interfaces in Domain  
✅ Implementations in Infrastructure  
✅ DTOs only across boundaries  
✅ Dapper for optimized reads  
✅ EF Core for persistence  

---

# 🛠 Setup Instructions

---

## 1. Clone Repository

```bash
git clone https://github.com/your-username/AuthPlatform.git
```

---

## 2. Open Solution

```bash
cd AuthPlatform
```

Open:

```text
AuthPlatform.sln
```

---

## 3. Configure Connection String

Update:

```json
appsettings.json
```

Example:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=.;Database=AuthPlatformDb;Trusted_Connection=True;TrustServerCertificate=True"
}
```

---

## 4. Apply Database Migrations

Set startup project:

```text
AuthPlatform.Api
```

Run:

```bash
dotnet ef database update
```

---

## 5. Run API

```bash
dotnet run --project AuthPlatform.Api
```

---

## 6. Run MVC

```bash
dotnet run --project AuthPlatform.Mvc
```

---

# 🔑 Default Authentication Flow

```text
MVC Login
   │
   ▼
API AuthController
   │
   ▼
JWT + Refresh Token
   │
   ▼
Stored in MVC Session
   │
   ▼
Bearer Token attached to API calls
```

---

# 📌 Key Features

- Enterprise-ready architecture
- Scalable modular design
- Hybrid EF + Dapper approach
- Advanced permission system
- Audit logging
- Login tracking
- RDLC reporting
- Secure session-based JWT handling
- Refresh token support

---

# 📈 Future Enhancements

- Multi-tenancy
- Redis caching
- CQRS + MediatR
- Event sourcing
- Background jobs
- API versioning
- OpenTelemetry tracing
- Docker support
- Kubernetes deployment
- Identity provider integration

---

# 🤝 Contribution Guidelines

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push branch
5. Open Pull Request

---

# 📄 License

MIT License

---

# 👨‍💻 Author

Developed using:
- ASP.NET Core
- Clean Architecture
- JWT Security
- EF Core
- Dapper
- RDLC Reporting
