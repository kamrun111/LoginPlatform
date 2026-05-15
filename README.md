# AuthPlatform

Enterprise-grade Authentication & Authorization Platform built with ASP.NET Core, Clean Architecture, JWT Authentication, EF Core, Dapper, and RDLC Reporting.

---

# Introduction

AuthPlatform is a reusable authentication and authorization platform designed for enterprise applications.

In many projects, developers spend a large amount of time rebuilding the same authentication features again and again:

- Login and logout
- JWT handling
- Refresh tokens
- Roles and permissions
- Audit logging
- Session handling
- Reporting
- Security setup

The goal of this project is to solve that problem by providing a clean, modular, and production-ready authentication foundation that can easily be integrated into different systems.

Instead of rebuilding authentication infrastructure for every application, developers can integrate AuthPlatform and focus on the actual business requirements of their projects.

This platform is designed to be:

- Reusable
- Scalable
- Secure
- Modular
- Enterprise-ready

AuthPlatform can be used in:

- ERP Systems
- HRM Systems
- School Management Systems
- Hospital Systems
- Finance Applications
- Inventory Systems
- SaaS Platforms
- Internal Enterprise Systems

The project also serves as:

- A reusable authentication module
- A Clean Architecture reference project
- A secure enterprise application foundation
- A learning resource for enterprise ASP.NET Core development

---

# Core Features

- JWT Authentication
- Refresh Tokens
- Role-Based Authorization
- User-Based Permissions
- Claims-Based Security
- Session-Based JWT Handling
- Audit Logging
- Login History Tracking
- EF Core + Dapper Hybrid Data Access
- RDLC Reporting
- PDF & Excel Export
- Secure Password Hashing
- Permission Middleware
- Clean Architecture Structure

---

# Architecture Overview

The solution follows **Clean Architecture** principles with clear separation between business logic, infrastructure, API, and presentation.

The authentication flow starts from the browser and moves through MVC, API, Application services, Domain rules, and finally Infrastructure and SQL Server.

```text
┌──────────────────────────────────────────┐
│                 Browser                  │
│------------------------------------------│
│ - User accesses Login Page               │
│ - Holds Secure HttpOnly Session Cookie   │
│ - Does NOT store JWT tokens              │
└────────────────────┬─────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────┐
│            AuthPlatform.Mvc              │
│        (UI – calls API via HTTP)         │
│------------------------------------------│
│ - Login / Logout UI                      │
│ - MVC Controllers & Views                │
│ - Stores JWT + Refresh Token in Session  │
│ - Calls API using Bearer Token           │
│ - Generates RDLC Reports                 │
└────────────────────┬─────────────────────┘
                     │ HTTP ONLY (no project reference)
                     ▼
┌──────────────────────────────────────────┐
│             AuthPlatform.Api             │
│      (Controllers, JWT, DI, etc.)       │
│------------------------------------------│
│ - Authentication                         │
│ - Authorization                          │
│ - Controllers                            │
│ - Middleware                             │
│ - Stateless API Layer                    │
└────────────────────┬─────────────────────┘
                     │ MUST reference Application
                     ▼
┌──────────────────────────────────────────┐
│         AuthPlatform.Application         │
│------------------------------------------│
│ - Services                               │
│ - Use Cases                              │
│ - DTOs                                   │
│ - Validators                             │
│ - Business Workflows                     │
└────────────────────┬─────────────────────┘
                     │ MUST reference Domain
                     ▼
┌──────────────────────────────────────────┐
│            AuthPlatform.Domain           │
│      (Entities, Interfaces, Core)        │
│------------------------------------------│
│ - Entities                               │
│ - Interfaces                             │
│ - Business Rules                         │
│ - Core Models                            │
└────────────────────┬─────────────────────┘
                     │ MUST reference Domain + Application
                     ▼
┌──────────────────────────────────────────┐
│       AuthPlatform.Infrastructure        │
│        (EF, Dapper, SQL, Repos)          │
│------------------------------------------│
│ - EF Core                                │
│ - Dapper                                 │
│ - Repository Implementations             │
│ - JWT Token Generator                    │
│ - Password Hasher                        │
│ - SQL Server Access                      │
└──────────────────────────────────────────┘
```

# Clean Architecture Rules

The project follows the main Clean Architecture principle:

> Inner layers never depend on outer layers.

## Dependency Flow

- Domain has no dependencies.
- Application depends only on Domain.
- Infrastructure depends on Domain and Application.
- API depends on Application and Infrastructure.
- MVC communicates with API through HTTP calls only.

## Important Rules

- MVC never accesses the database directly.
- API remains stateless for access-token authentication.
- Repository interfaces are defined in Domain.
- Repository implementations live in Infrastructure.
- DTOs are used across boundaries.
- EF Core is mainly used for transactional operations.
- Dapper is used for optimized reads, reports, and high-performance queries.

---

# Solution Structure

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

# Project Responsibilities

## 1. AuthPlatform.Domain

Contains:

- Entities
- Repository interfaces
- Business rules
- Shared base models

Example:

```text
Entities/
Interfaces/
Common/
```

---

## 2. AuthPlatform.Application

Contains:

- Application services
- DTOs
- Validators
- Mapping profiles
- Business workflows
- Service interfaces

Example:

```text
DTOs/
Services/
Interfaces/
Validators/
Mappings/
```

---

## 3. AuthPlatform.Infrastructure

Contains:

- EF Core DbContext
- EF Core repositories
- Dapper repositories
- JWT generation
- Password hashing
- SQL scripts
- Repository implementations

Example:

```text
Persistence/
EF/
Dapper/
Security/
SQL/
```

---

## 4. AuthPlatform.Api

Contains:

- REST APIs
- Authentication pipeline
- Middleware
- Authorization
- Filters

Example:

```text
Controllers/
Middleware/
Filters/
```

---

## 5. AuthPlatform.Mvc

Contains:

- MVC UI
- Views
- Session-based token handling
- API communication
- RDLC reporting
- PDF/Excel export

Example:

```text
Controllers/
Views/
Reports/
Services/
Session/
```

---

# Authentication Flow

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

# Authorization Model

The system supports both:

## Role-Based Permissions

Example:

```text
Admin
 ├── User.Create
 ├── User.Edit
 └── Report.View
```

## User-Based Permissions

Example:

```text
John Doe
 ├── Expenditure.Approve
 └── Expenditure.Edit
```

## Effective Permission Resolution

A user’s final permissions are calculated from:

```text
Role Permissions
+ Direct User Permissions
= Effective Permissions
```

This provides flexibility for enterprise systems where users may require additional permissions beyond their assigned roles.

---

# Database Design

## Core Authentication Tables

| Table | Purpose |
|---|---|
| AuthUsers | Stores system users |
| AuthRoles | Stores user roles |
| AuthPermissions | Stores available permissions |
| AuthUserPermissions | Stores user-level permissions |
| AuthRolePermissions | Stores role-level permissions |
| AuthLoginHistories | Tracks login attempts |
| AuthAuditActivities | Stores audit logs |
| AuthRefreshTokens | Stores refresh tokens |

---

# Data Access Strategy

## EF Core

Used for:

- Create
- Update
- Delete
- Entity tracking
- Migrations
- Transactional operations

## Dapper

Used for:

- Reports
- Read-heavy queries
- Dashboard summaries
- Permission queries
- Stored procedures
- Fast lookups
- Optimized SQL operations

---

# Security Design

## API Security

- Stateless JWT authentication
- Claims-based authorization
- Permission middleware
- Authorization policies
- Refresh token validation
- Token rotation support
- Token revocation support

## MVC Security

- JWT tokens are NOT stored in browser localStorage
- JWT tokens are NOT stored in browser sessionStorage
- Tokens are stored only in server-side session
- Browser receives only Secure HttpOnly cookies

## Password Security

Supported hashing methods:

- PBKDF2
- BCrypt

## Audit Logging

The system tracks:

- User actions
- Login attempts
- Role assignments
- Permission changes
- Sensitive operations

---

# RDLC Reporting

The system uses RDLC reporting for generating server-side reports.

## Reports Without Parameters

- Active Users Report
- Dashboard Summary Report

## Reports With Parameters

- User Permission Report
- Role Permission Report
- Login History Report
- Audit Activity Report

## Export Formats

- PDF
- Excel

---

# Technology Stack

## Backend

- ASP.NET Core Web API
- ASP.NET Core MVC
- Entity Framework Core
- Dapper
- SQL Server

## Security

- JWT Authentication
- Refresh Tokens
- Claims-Based Authorization
- Permission-Based Authorization
- PBKDF2 / BCrypt Password Hashing

## Reporting

- RDLC Reports
- PDF Export
- Excel Export

## Validation & Mapping

- FluentValidation
- AutoMapper

---

# Setup Instructions

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

Update `appsettings.json` in `AuthPlatform.Api`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=.;Database=AuthPlatformDb;Trusted_Connection=True;TrustServerCertificate=True"
}
```

---

## 4. Install EF Core Tool

```bash
dotnet tool install --global dotnet-ef
```

---

## 5. Apply Database Migrations

```bash
dotnet ef migrations add InitialCreate --project AuthPlatform.Infrastructure --startup-project AuthPlatform.Api
```

```bash
dotnet ef database update --project AuthPlatform.Infrastructure --startup-project AuthPlatform.Api
```

---

## 6. Run API

```bash
dotnet run --project AuthPlatform.Api
```

---

## 7. Run MVC

```bash
dotnet run --project AuthPlatform.Mvc
```

---

# Future Enhancements

Planned improvements include:

- Multi-tenancy
- Redis caching
- API versioning
- CQRS + MediatR
- Background jobs
- OpenTelemetry tracing
- Docker support
- Kubernetes deployment
- Permission caching
- Distributed caching
- Identity provider integration

---

# Contribution Guidelines

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push your branch
5. Open a Pull Request

---

# License

MIT License

---

# Author

Developed using:

- ASP.NET Core
- Clean Architecture
- EF Core
- Dapper
- JWT Security
- RDLC Reporting
