# API Reference Guide

Complete API documentation for the Employee Management Full-Stack Application.

## Base URL

```
http://localhost:8080/api
```

## Authentication

All endpoints (except `/auth/**`) require a JWT token in the `Authorization` header:

```
Authorization: Bearer <jwt_token>
```

### Obtain Token

**Request:**
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "username": "john",
    "email": "john@example.com",
    "roles": ["ROLE_ADMIN"]
  }
}
```

**Status Code:** `200 OK`

---

## Endpoints

### Authentication Endpoints

#### Register New User

```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "newuser",
  "email": "newuser@example.com",
  "password": "SecurePass123!",
  "confirmPassword": "SecurePass123!"
}
```

**Response:**
```json
{
  "message": "User registered successfully",
  "userId": 5
}
```

**Status Code:** `201 Created`

#### Verify Email

```http
POST /api/auth/verify-email
Content-Type: application/json

{
  "email": "user@example.com",
  "verificationCode": "123456"
}
```

**Response:**
```json
{
  "message": "Email verified successfully"
}
```

**Status Code:** `200 OK`

#### Reset Password

```http
POST /api/auth/reset-password
Content-Type: application/json

{
  "email": "user@example.com",
  "newPassword": "NewSecurePass123!",
  "resetToken": "reset_token_12345"
}
```

**Response:**
```json
{
  "message": "Password reset successfully"
}
```

**Status Code:** `200 OK`

---

### Employee Endpoints

#### Get All Employees

```http
GET /api/employees?page=0&size=20&sort=name,asc
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "content": [
    {
      "id": 1,
      "name": "John Smith",
      "email": "john@example.com",
      "phone": "555-0101",
      "address": "123 Oak Street, New York, NY 10001",
      "position": "Software Engineer",
      "salary": 125000.00,
      "joinDate": "2023-01-15",
      "department": {
        "id": 1,
        "name": "Engineering"
      },
      "createdAt": "2023-01-15T10:30:00Z",
      "updatedAt": "2024-11-17T14:20:00Z"
    },
    {
      "id": 2,
      "name": "Sarah Johnson",
      "email": "sarah@example.com",
      "phone": "555-0102",
      "address": "456 Elm Avenue, Boston, MA 02101",
      "position": "Product Manager",
      "salary": 118500.00,
      "joinDate": "2022-06-20",
      "department": {
        "id": 2,
        "name": "Product"
      },
      "createdAt": "2022-06-20T09:15:00Z",
      "updatedAt": "2024-11-17T12:45:00Z"
    }
  ],
  "pageable": {
    "pageNumber": 0,
    "pageSize": 20,
    "totalElements": 10,
    "totalPages": 1
  }
}
```

**Status Code:** `200 OK`

#### Get Employee by ID

```http
GET /api/employees/1
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "id": 1,
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "555-0101",
  "address": "123 Oak Street, New York, NY 10001",
  "position": "Software Engineer",
  "salary": 125000.00,
  "joinDate": "2023-01-15",
  "department": {
    "id": 1,
    "name": "Engineering"
  },
  "createdAt": "2023-01-15T10:30:00Z",
  "updatedAt": "2024-11-17T14:20:00Z"
}
```

**Status Code:** `200 OK`

#### Create Employee

```http
POST /api/employees
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "phone": "555-0150",
  "address": "789 Pine Road, Los Angeles, CA 90001",
  "position": "Senior Engineer",
  "salary": 145000.00,
  "joinDate": "2025-01-01",
  "departmentId": 1
}
```

**Response:**
```json
{
  "id": 11,
  "name": "Jane Doe",
  "email": "jane@example.com",
  "phone": "555-0150",
  "address": "789 Pine Road, Los Angeles, CA 90001",
  "position": "Senior Engineer",
  "salary": 145000.00,
  "joinDate": "2025-01-01",
  "department": {
    "id": 1,
    "name": "Engineering"
  },
  "createdAt": "2024-11-17T15:30:00Z",
  "updatedAt": "2024-11-17T15:30:00Z"
}
```

**Status Code:** `201 Created`

#### Update Employee

```http
PUT /api/employees/1
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "555-0101",
  "address": "123 Oak Street, New York, NY 10001",
  "position": "Senior Software Engineer",
  "salary": 145000.00,
  "departmentId": 1
}
```

**Response:**
```json
{
  "id": 1,
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "555-0101",
  "address": "123 Oak Street, New York, NY 10001",
  "position": "Senior Software Engineer",
  "salary": 145000.00,
  "joinDate": "2023-01-15",
  "department": {
    "id": 1,
    "name": "Engineering"
  },
  "createdAt": "2023-01-15T10:30:00Z",
  "updatedAt": "2024-11-17T16:00:00Z"
}
```

**Status Code:** `200 OK`

#### Delete Employee

```http
DELETE /api/employees/1
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "message": "Employee deleted successfully"
}
```

**Status Code:** `204 No Content`

#### Search Employees

```http
GET /api/employees/search?query=John&department=Engineering
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "content": [
    {
      "id": 1,
      "name": "John Smith",
      "email": "john@example.com",
      ...
    }
  ]
}
```

**Status Code:** `200 OK`

---

### Department Endpoints

#### Get All Departments

```http
GET /api/departments
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "content": [
    {
      "id": 1,
      "name": "Engineering",
      "description": "Software Engineering Department",
      "location": "San Francisco, CA",
      "manager": "Robert Garcia",
      "budget": 12500000.00,
      "employeeCount": 45,
      "createdAt": "2023-01-01T00:00:00Z",
      "updatedAt": "2024-11-17T10:00:00Z"
    },
    {
      "id": 2,
      "name": "Product",
      "description": "Product Management Department",
      "location": "San Francisco, CA",
      "manager": "Lisa Anderson",
      "budget": 4200000.00,
      "employeeCount": 18,
      "createdAt": "2023-01-01T00:00:00Z",
      "updatedAt": "2024-11-17T10:00:00Z"
    }
  ]
}
```

**Status Code:** `200 OK`

#### Get Department by ID

```http
GET /api/departments/1
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "id": 1,
  "name": "Engineering",
  "description": "Software Engineering Department",
  "location": "San Francisco, CA",
  "manager": "Robert Garcia",
  "budget": 12500000.00,
  "employeeCount": 45,
  "employees": [
    {
      "id": 1,
      "name": "John Smith",
      "position": "Software Engineer"
    }
  ],
  "createdAt": "2023-01-01T00:00:00Z",
  "updatedAt": "2024-11-17T10:00:00Z"
}
```

**Status Code:** `200 OK`

#### Create Department

```http
POST /api/departments
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "name": "Innovation Lab",
  "description": "Research and Innovation Department",
  "location": "Palo Alto, CA",
  "manager": "Dr. Michael Chen",
  "budget": 5000000.00
}
```

**Response:**
```json
{
  "id": 8,
  "name": "Innovation Lab",
  "description": "Research and Innovation Department",
  "location": "Palo Alto, CA",
  "manager": "Dr. Michael Chen",
  "budget": 5000000.00,
  "employeeCount": 0,
  "createdAt": "2024-11-17T16:30:00Z",
  "updatedAt": "2024-11-17T16:30:00Z"
}
```

**Status Code:** `201 Created`

#### Update Department

```http
PUT /api/departments/1
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "name": "Engineering",
  "description": "Software Engineering Department",
  "location": "San Francisco, CA",
  "manager": "Robert Garcia",
  "budget": 13000000.00
}
```

**Response:**
```json
{
  "id": 1,
  "name": "Engineering",
  "description": "Software Engineering Department",
  "location": "San Francisco, CA",
  "manager": "Robert Garcia",
  "budget": 13000000.00,
  "employeeCount": 45,
  "createdAt": "2023-01-01T00:00:00Z",
  "updatedAt": "2024-11-17T16:45:00Z"
}
```

**Status Code:** `200 OK`

#### Delete Department

```http
DELETE /api/departments/1
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "message": "Department deleted successfully"
}
```

**Status Code:** `204 No Content`

---

### Dashboard Endpoints

#### Get Dashboard Statistics

```http
GET /api/dashboard/statistics
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "totalEmployees": 250,
  "totalDepartments": 7,
  "averageSalary": 112500.50,
  "highestSalary": 180000.00,
  "lowestSalary": 45000.00,
  "employeesByDepartment": {
    "Engineering": 45,
    "Product": 18,
    "Sales": 35,
    "HR": 12,
    "Marketing": 22,
    "Finance": 20,
    "Operations": 28
  },
  "departmentBudgets": {
    "Engineering": 12500000.00,
    "Product": 4200000.00,
    "Sales": 7800000.00,
    "HR": 2100000.00,
    "Marketing": 3900000.00,
    "Finance": 3200000.00,
    "Operations": 4500000.00
  },
  "recentHires": [
    {
      "id": 11,
      "name": "Jane Doe",
      "position": "Senior Engineer",
      "joinDate": "2025-01-01"
    }
  ]
}
```

**Status Code:** `200 OK`

---

## Error Responses

### Bad Request (400)

```json
{
  "error": "Bad Request",
  "message": "Invalid input parameters",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees"
}
```

### Unauthorized (401)

```json
{
  "error": "Unauthorized",
  "message": "JWT token is invalid or expired",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees"
}
```

### Forbidden (403)

```json
{
  "error": "Forbidden",
  "message": "User does not have required role",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees/delete"
}
```

### Not Found (404)

```json
{
  "error": "Not Found",
  "message": "Employee with id 999 not found",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees/999"
}
```

### Conflict (409)

```json
{
  "error": "Conflict",
  "message": "Email already exists",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees"
}
```

### Internal Server Error (500)

```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred",
  "timestamp": "2024-11-17T15:30:00Z",
  "path": "/api/employees"
}
```

---

## HTTP Status Codes

| Code | Meaning | Use Case |
|------|---------|----------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Invalid input parameters |
| 401 | Unauthorized | Missing or invalid JWT token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource conflict (e.g., duplicate email) |
| 500 | Server Error | Internal server error |

---

## Rate Limiting

API requests are rate-limited to:
- **100 requests per minute** per IP address for authenticated users
- **10 requests per minute** for unauthenticated requests

Rate limit headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1700245800
```

---

## Pagination

Paginated endpoints support the following query parameters:

| Parameter | Type | Default | Example |
|-----------|------|---------|---------|
| `page` | integer | 0 | `?page=1` |
| `size` | integer | 20 | `?size=50` |
| `sort` | string | `id,desc` | `?sort=name,asc` |

---

## OpenAPI/Swagger Documentation

Access interactive API documentation at:

```
http://localhost:8080/swagger-ui.html
```

Download OpenAPI specification:

```
http://localhost:8080/v3/api-docs
```

---

**API Version:** 1.0.0  
**Last Updated:** November 17, 2025  
**Maintainer:** Kodati Sai Teja  
**GitHub:** https://github.com/TEJA6777
