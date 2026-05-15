# AuthPlatform

Enterprise-grade Authentication & Authorization Platform built with **ASP.NET Core**, **Clean Architecture**, **JWT Authentication**, **Role & Permission Authorization**, **EF Core**, **Dapper**, and **RDLC Reporting**.

---

# 🚀 Introduction

AuthPlatform is a reusable enterprise-grade Authentication & Authorization system built with **ASP.NET Core** and **Clean Architecture**.

The purpose of this project is to provide a complete ready-to-integrate authentication module so developers and companies do not need to build login, authorization, permissions, JWT handling, audit logging, reporting, and security infrastructure from scratch for every new project.

Instead of spending time designing authentication repeatedly, developers can simply integrate AuthPlatform into their own business systems and focus entirely on their core business requirements.

This platform is designed to be:

- Reusable
- Scalable
- Secure
- Modular
- Enterprise-ready

The system supports:

- JWT Authentication
- Refresh Tokens
- Role-based Authorization
- User-based Permissions
- Claims-based Security
- Audit Logging
- Login History Tracking
- Session-based JWT Management
- RDLC Reporting
- EF Core + Dapper Hybrid Data Access

AuthPlatform can be integrated into:

- ERP Systems
- HRM Systems
- School Management Systems
- Hospital Systems
- Finance Applications
- Inventory Systems
- SaaS Platforms
- Enterprise Internal Systems

Developers can use this project as:

- A production-ready authentication server
- A reusable authentication module
- A Clean Architecture reference project
- A secure enterprise application foundation

The architecture is designed in a way that:

- MVC never directly accesses the database
- API remains stateless
- Security is centralized
- Permissions are flexible
- Reporting is modular
- Future modules can easily be added

This allows teams to plug the authentication module into any enterprise application without redesigning security every time.

---

# 🏗 Architecture

The solution follows **Clean Architecture** with strict dependency flow.

The system starts from the **user login flow**, because this project is mainly an enterprise authentication and authorization platform.

```text
┌──────────────────────────────────────────┐
│                Browser                   │
│------------------------------------------│
│ - User accesses Login Page               │
│ - Holds Secure HttpOnly Session Cookie   │
│ - Does NOT store JWT tokens              │
└────────────────────┬─────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────┐
│            AuthPlatform.Mvc              │
│------------------------------------------│
│ - Login / Logout UI                      │
│ - MVC Controllers & Views                │
│ - Stores JWT + Refresh Token in Session  │
│ - Calls API using Bearer Token           │
│ - Generates RDLC Reports                 │
└────────────────────┬─────────────────────┘
                     │ HTTP API Calls
                     ▼
┌──────────────────────────────────────────┐
│             AuthPlatform.Api             │
│------------------------------------------│
│ - AuthController                         │
│ - RoleController                         │
│ - PermissionController                   │
│ - JWT Authentication                     │
│ - Permission Middleware                  │
│ - Stateless Authorization                │
└────────────────────┬─────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────┐
│         AuthPlatform.Application         │
│------------------------------------------│
│ - Auth Services                          │
│ - Permission Use Cases                   │
│ - Role Services                          │
│ - DTOs                                   │
│ - Validators                             │
│ - Application Interfaces                 │
└────────────────────┬─────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────┐
│            AuthPlatform.Domain           │
│------------------------------------------│
│ - AuthUser                               │
│ - AuthRole                               │
│ - AuthPermission                         │
│ - AuthUserPermission                     │
│ - AuthRolePermission                     │
│ - AuthLoginHistory                       │
│ - AuthAuditActivity                      │
│ - Repository Interfaces                  │
│ - Business Rules                         │
└────────────────────┬─────────────────────┘
                     ▲
                     │ Implements interfaces
                     │
┌──────────────────────────────────────────┐
│         AuthPlatform.Infrastructure      │
│------------------------------------------│
│ - EF Core DbContext                      │
│ - EF Core Repositories                   │
│ - Dapper Repositories                    │
│ - Password Hasher                        │
│ - JWT Token Generator                    │
│ - Refresh Token Storage                  │
│ - SQL Views / Stored Procedures          │
└──────────────────────────────────────────┘
```

---

# 🔄 Architecture Flow

```text
Browser
   │
   ▼
MVC Login Page
   │
   ▼
API AuthController
   │
   ▼
Application AuthService
   │
   ▼
Domain Auth Rules
   │
   ▼
Infrastructure EF Core / Dapper
   │
   ▼
SQL Server
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

# 🧠 Clean Architecture Rules

---

## Dependency Rule

```text
AuthPlatform.Domain
   ↑
AuthPlatform.Application
   ↑
AuthPlatform.Infrastructure
   ↑
AuthPlatform.Api
   ↑
AuthPlatform.Mvc
```

The inner layers do not depend on outer layers.

- Domain does not know about Application, Infrastructure, API, or MVC
- Application depends only on Domain
- Infrastructure implements interfaces
- API uses Application services
- MVC communicates only with API

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
