# Employee Management System – Backend Engineering Assignment

## Overview

This repository contains the **backend engineering assignment specification** for developers joining the team.

The goal of this assignment is to evaluate a candidate’s ability to design and implement a backend service using **modern .NET architecture and enterprise-level best practices**.

Candidates are expected to implement a simplified **Employee Management System (EMS)** that demonstrates:

- Clean layered architecture
- Proper API design
- Authentication & Authorization
- Database design
- Business logic implementation
- Salary disbursement
- Reporting system
- Logging and auditing

This assignment reflects the **architecture and engineering standards used in production systems**.

---

# Assignment Objective

The system must support the following capabilities:

- Employee Management
- Department Management
- Designation Management
- Salary Disbursement
- Reporting
- Authentication
- Role Based Access Control
- Logging

Candidates are responsible for designing the **database schema, entity relationships, and data structure**.

The focus of this assignment is not only functionality but also **architecture, design decisions, and code quality**.

---

# Technology Stack

## Backend

- .NET 9
- ASP.NET Core Web API
- Entity Framework Core

## Databases

The system must use **two separate databases**.

| Database | Purpose |
|--------|--------|
| MSSQL | Main application data |
| PostgreSQL | Logging database |

---

# Architecture

The system must follow **clean layered architecture**.

```
EmployeeManagementSystem

EmployeeManagement.Domain
EmployeeManagement.Application
EmployeeManagement.Persistence
EmployeeManagement.Infrastructure
EmployeeManagement.Shared
EmployeeManagement.Configuration
EmployeeManagement.WebAPI
```

Each layer must have clearly defined responsibilities.

---

# Architecture Diagram

```
Client
   │
   ▼
WebAPI Layer
   │
   ▼
Application Layer
   │
   ▼
Persistence Layer
   │
   ├── MSSQL (Main Data)
   │
   └── PostgreSQL (Logs)
```

Layer Responsibilities

| Layer | Responsibility |
|------|------|
| WebAPI | Controllers and request handling |
| Application | Business logic |
| Persistence | Database interaction |
| Domain | Core business entities |
| Infrastructure | External services |
| Shared | Utilities and response models |
| Configuration | Dependency injection and middleware |

---

# Domain Design

Candidates must design domain entities representing:

- Employees
- Departments
- Designations
- Salary Disbursement
- Users
- Roles
- Permissions

Entity relationships must be designed logically based on the system requirements.

The domain layer should contain:

```
Domain
 ├── Models
 ├── Enums
 └── Common
```

Rules

- No database logic
- No framework dependencies
- Only domain entities and rules

---

# Application Layer

The application layer must contain:

- DTOs
- Services
- Validators
- Mappers
- Business logic

Structure example

```
Application
 ├── DTO
 │     ├── Request
 │     └── Response
 │
 ├── Services
 │     ├── Interfaces
 │     └── Implementations
 │
 ├── Validators
 └── Mappers
```

Expected services may include:

- Authentication service
- Employee management service
- Department service
- Designation service
- Salary service
- Reporting service

---

# Persistence Layer

Responsible for database access.

```
Persistence
 ├── DbContext
 ├── Entity Configurations
 ├── Repositories
 ├── UnitOfWork
 └── Database
```

Responsibilities

- Entity Framework configurations
- Database interaction
- Repository pattern
- Unit of Work implementation

Database design decisions must be made by the candidate.

---

# Infrastructure Layer

Responsible for integrations and external services.

Examples may include:

- Logging service
- Email service
- Notification service

---

# Shared Layer

Contains reusable components.

```
Shared
 ├── Config
 ├── DTO
 ├── Exceptions
 ├── Utilities
 └── API Response models
```

---

# API Response Standard

All API responses must follow a **consistent response structure**.

Example Response

```
{
  "isSuccess": true,
  "statusCode": "200",
  "message": "Employees retrieved successfully",
  "data": {},
  "errors": []
}
```

Example Model

```
public class StandardResponse
{
    public bool IsSuccess { get; set; }
    public string StatusCode { get; set; }
    public string Message { get; set; }
    public object? Data { get; set; }
    public List<ErrorDetails>? Errors { get; set; }
}
```

Helper classes may be used to standardize API responses.

---

# Authentication

JWT authentication must be implemented.

Example login endpoint:

```
POST /api/auth/login
```

The authentication system must issue a **JWT token** upon successful login.

---

# Logout

A logout endpoint must be implemented.

```
POST /api/auth/logout
```

Requirements

- Logout activity must be logged
- Client must discard stored token

---

# RBAC (Database Based)

Authorization must be **database-driven**.

Do not rely solely on framework role attributes.

Instead implement a permission-based access system using database entities.

Conceptual flow

```
User → Role → Permission → API Access
```

The authorization system should be flexible enough to allow role-permission mapping.

---

# Core Functional Modules

The system must include the following modules:

- Department Management
- Designation Management
- Employee Management
- Salary Disbursement
- Reporting
- Logging

Exact database schema and entity fields must be designed by the candidate.

---

# Employee Management

The system must support:

- Creating employees
- Updating employee information
- Retrieving employee data
- Deactivating employees

Employee records must be associated with a department and designation.

---

# Department Management

The system must support:

- Creating departments
- Updating department information
- Listing departments
- Removing departments

---

# Designation Management

The system must support:

- Creating designations
- Updating designations
- Listing designations

---

# Salary Disbursement

The system must support salary disbursement for employees.

Salary must be handled on a **monthly basis**.

The system should support:

- Recording salary payments
- Tracking salary history
- Managing salary adjustments

Salary calculation logic must be implemented in the application layer.

---

# Reporting Layer

The system must implement a **reporting layer** capable of generating aggregated data.

Example reports include:

- Total salary disbursed
- Salary distribution by department
- Monthly salary summary
- Employee salary history

Report generation logic should be separated from core business logic.

---

# Logging System

All system logs must be stored in **PostgreSQL**.

Logs should capture events such as:

- Login
- Logout
- Entity creation
- Entity update
- Entity deletion
- System errors

Log records should include sufficient metadata to trace system activity.

---

# Validation

Request validation must be implemented using **FluentValidation**.

Validation examples include:

- Email format validation
- Required fields
- Logical data constraints
- Business rule validation

---

# Error Handling

A **global exception handling middleware** must be implemented.

All errors must return a structured API response.

Example

```
{
  "isSuccess": false,
  "statusCode": "404",
  "message": "Resource not found",
  "errors": []
}
```

---

# Deliverables

Candidates must submit:

1. Source code repository
2. Database migrations or scripts
3. README with setup instructions

The README must contain:

- Project setup instructions
- Database configuration steps
- Running the application
- API testing instructions

---

# Evaluation Criteria

| Criteria | Priority |
|------|------|
Architecture | High |
Code Quality | High |
Authentication | High |
Database Design | High |
Reporting Implementation | Medium |
Logging System | Medium |
Documentation | Medium |

---

# Bonus (Optional)

Optional improvements may include:

- Pagination
- Filtering
- CSV export for reports
