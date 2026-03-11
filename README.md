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

This assignment mirrors the **architecture and engineering standards used in production systems**.

---

# Assignment Objective

The system must manage:

- Employees
- Departments
- Designations
- Salary Disbursement
- Reporting
- Authentication
- Role Based Access Control
- Logging

Candidates must demonstrate:

- Proper architecture
- Clean code
- Consistent API contracts
- Proper error handling
- Multi-database management

---

# Technology Stack

## Backend

- .NET 9
- ASP.NET Core Web API
- Entity Framework Core

## Databases

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
| WebAPI | Controllers, HTTP handling |
| Application | Business logic |
| Persistence | Database access |
| Domain | Entities |
| Infrastructure | External services |
| Shared | Utilities & responses |
| Configuration | DI & middleware |

---

# Domain Layer

Contains core business entities.

Example structure

```
Domain
 ├── Models
 │     ├── Employee
 │     ├── Department
 │     ├── Designation
 │     ├── SalaryDisbursement
 │     ├── User
 │     └── Role
 │
 ├── Enum
 │     └── UserRole
 │
 └── Common
       ├── BaseEntity
       └── BaseAuditEntity
```

Rules

- No framework dependency
- No database logic

---

# Application Layer

Contains:

- Business logic
- DTOs
- Validators
- Services
- Mappers

Structure

```
Application
 ├── Dto
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

Example Services

```
AuthService
EmployeeService
DepartmentService
DesignationService
SalaryService
ReportService
```

---

# Persistence Layer

Handles database interaction.

```
Persistence
 ├── DbContext
 ├── Configurations
 ├── Repositories
 │     ├── Interfaces
 │     └── Implementations
 ├── UnitOfWork
 └── Database
```

---

# Infrastructure Layer

Responsible for external integrations.

Examples

```
Infrastructure
 ├── LoggingService
 └── EmailService
```

---

# Shared Layer

Contains shared utilities.

```
Shared
 ├── Config
 ├── DTO
 ├── Exceptions
 ├── Utils
 └── ApiResponse
```

---

# API Response Standard

All APIs must return responses using **StandardResponse**.

Example Structure

```
{
  "isSuccess": true,
  "statusCode": "200",
  "message": "Employees retrieved successfully",
  "data": {},
  "errors": []
}
```

StandardResponse Model

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

Use helper methods for generating responses.

Example

```
ApiResponseHelper.Success("Employees retrieved", data)
ApiResponseHelper.Created("Employee", employee)
ApiResponseHelper.ValidationError(...)
```

---

# Authentication

JWT based authentication must be implemented.

Login Endpoint

```
POST /api/auth/login
```

Request

```
{
  "email": "admin@company.com",
  "password": "password"
}
```

Response

```
{
  "isSuccess": true,
  "statusCode": "200",
  "message": "Login successful",
  "data": {
    "accessToken": "jwt-token"
  }
}
```

---

# Logout

Endpoint

```
POST /api/auth/logout
```

Requirements

- Logout event must be logged
- Token must be invalidated client-side

---

# RBAC (Database Based)

Authorization must be **database driven**.

Do NOT use:

```
[Authorize(Roles="Admin")]
```

Instead implement:

```
Roles Table
Permissions Table
RolePermissionMappings
```

Flow

```
User -> Role -> Permissions -> API Access
```

Tables

```
Users
Roles
Permissions
RolePermissionMappings
```

Permission Example

| Role | Permission |
|-----|------|
Admin | full access |
HR | employee management |
Manager | reporting |
Employee | view own profile |

---

# Core Modules

The system must contain:

- Department Management
- Designation Management
- Employee Management
- Salary Disbursement
- Reporting
- Logging

---

# Employee Module

Fields

| Field | Type |
|-----|------|
Id | Guid |
FirstName | string |
LastName | string |
Email | string |
Phone | string |
DepartmentId | Guid |
DesignationId | Guid |
Salary | decimal |
JoinDate | date |
IsActive | bool |

Endpoints

```
POST /api/employees
GET /api/employees
GET /api/employees/{id}
PUT /api/employees/{id}
DELETE /api/employees/{id}
```

---

# Department Module

Endpoints

```
POST /api/departments
GET /api/departments
PUT /api/departments/{id}
DELETE /api/departments/{id}
```

---

# Designation Module

Endpoints

```
POST /api/designations
GET /api/designations
PUT /api/designations/{id}
DELETE /api/designations/{id}
```

---

# Salary Disbursement

Entity

```
SalaryDisbursement
```

Fields

| Field | Type |
|------|------|
Id | Guid |
EmployeeId | Guid |
SalaryMonth | int |
SalaryYear | int |
BaseSalary | decimal |
Bonus | decimal |
Deduction | decimal |
NetSalary | decimal |
PaidDate | datetime |
Status | Pending / Paid |

Formula

```
NetSalary = BaseSalary + Bonus - Deduction
```

---

# Salary APIs

```
POST /api/salaries/disburse
GET /api/salaries/employee/{employeeId}
GET /api/salaries/monthly
```

---

# Reporting Layer

Reports required

Total Salary Paid

```
GET /api/reports/salary/total
```

Salary By Department

```
GET /api/reports/salary/by-department
```

Monthly Salary Report

```
GET /api/reports/salary/monthly
```

Employee Salary Report

```
GET /api/reports/salary/employee/{employeeId}
```

---

# ER Diagram

```
Departments
   │
   ├──< Employees >── Designations
   │
   └── SalaryDisbursements

Users
  │
  └── Roles
        │
        └── RolePermissions
```

---

# Logging System

Logs must be stored in **PostgreSQL**.

Log Types

- Login
- Logout
- Entity Create
- Entity Update
- Entity Delete
- Errors

Log Fields

```
Id
UserId
Action
Entity
Description
Timestamp
```

Example

```
Employee created by Admin
Salary disbursed for March
Login attempt
```

---

# Validation

Use **FluentValidation**

Examples

- Email must be valid
- Salary must be positive
- Department must exist
- Only one salary disbursement per employee per month

---

# Error Handling

Global exception middleware must be implemented.

Example response

```
{
  "isSuccess": false,
  "statusCode": "404",
  "message": "Employee not found",
  "errors": []
}
```

---

# Database Tables

Main DB (MSSQL)

```
Employees
Departments
Designations
Users
Roles
Permissions
RolePermissionMappings
SalaryDisbursements
```

Logging DB (PostgreSQL)

```
ApplicationLogs
LoginLogs
```

---

# Deliverables

Engineers must submit

1. Source code repository
2. Database migrations or SQL scripts
3. README with setup instructions

README must include

- Setup instructions
- Database configuration
- Running the project
- API examples

---

# Evaluation Criteria

| Criteria | Priority |
|------|------|
Architecture | High |
Code Quality | High |
Authentication | High |
Database Design | High |
Reporting | Medium |
Logging | Medium |
Documentation | Medium |

---

# Bonus (Optional)

Optional features

- Pagination
- Filtering
- CSV salary export
