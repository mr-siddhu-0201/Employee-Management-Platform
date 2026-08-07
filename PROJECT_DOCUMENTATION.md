# Employee Management Full-Stack Application - Comprehensive Documentation

## Table of Contents

1. [Project Overview](#project-overview)
2. [Purpose and Use Cases](#purpose-and-use-cases)
3. [Architecture](#architecture)
4. [Technology Stack](#technology-stack)
5. [Directory Structure & File Descriptions](#directory-structure--file-descriptions)
6. [Frontend Components](#frontend-components)
7. [Backend Structure](#backend-structure)
8. [Database Schema](#database-schema)
9. [API Endpoints](#api-endpoints)
10. [Deployment & DevOps](#deployment--devops)
11. [Getting Started](#getting-started)
12. [Troubleshooting](#troubleshooting)
13. [Testing](#testing)
14. [Future Enhancements](#future-enhancements)

---

## Project Overview

The **Employee Management Full-Stack Application** is a modern, enterprise-grade system designed to manage employee records, departmental information, and organizational hierarchies. It demonstrates the integration of contemporary React-based frontend technologies with a robust Java Spring Boot backend, providing a complete CRUD (Create, Read, Update, Delete) solution for HR management.

### Key Features

- ✅ **Complete Employee Management** - Add, edit, delete, and view employee records
- ✅ **Department Management** - Organize employees by departments
- ✅ **User Authentication** - Secure login and registration with JWT tokens
- ✅ **Real-time Dashboard** - Visual analytics with charts and metrics
- ✅ **Responsive Design** - Works seamlessly on desktop, tablet, and mobile
- ✅ **REST API** - Well-documented API endpoints with Swagger/OpenAPI
- ✅ **Containerization** - Docker and Kubernetes ready
- ✅ **CI/CD Pipeline** - Jenkins integration for automated deployment

---

## Purpose and Use Cases

### Business Purpose

This application serves as a complete HR management solution for organizations of any size to:

1. **Employee Lifecycle Management**
   - Onboard new employees
   - Track employee information (name, email, phone, address, etc.)
   - Monitor employee tenure and roles
   - Archive historical employee data

2. **Department Organization**
   - Create and manage departments
   - Assign employees to departments
   - Track departmental structure and hierarchy
   - Monitor department-level metrics

3. **Analytics & Reporting**
   - View employee statistics (total count, average age)
   - Track employee growth trends over time
   - Analyze age distribution across the organization
   - Generate employee reports

4. **Data Security**
   - Secure authentication and authorization
   - Role-based access control (future enhancement)
   - Password hashing and JWT token-based sessions
   - Audit logging capabilities

### Real-World Scenarios

**Scenario 1: Small Startup**
- HR manager uses the app to manage 50-100 employees
- Creates departments (Engineering, Sales, HR, etc.)
- Uses dashboard to monitor team growth
- Generates monthly reports

**Scenario 2: Enterprise Organization**
- Deployed across multiple locations
- Kubernetes orchestration for high availability
- Multiple user roles with different permissions
- Integration with existing HR systems

**Scenario 3: Educational Institution**
- Manage faculty and staff
- Organize by departments (Science, Arts, Engineering)
- Track staff hierarchy and tenure

---

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                          Frontend (React)                        │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Presentation Layer: Components, Pages, Forms, Charts       │ │
│  │ - Login/Register Components                                │ │
│  │ - Employee Management UI                                   │ │
│  │ - Dashboard with Analytics                                 │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕ (REST API)
┌─────────────────────────────────────────────────────────────────┐
│                    API Gateway / Backend (Spring Boot)           │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Controller Layer: REST Endpoints                           │ │
│  │ - EmployeeController: /api/employees                       │ │
│  │ - DepartmentController: /api/departments                   │ │
│  │ - AuthController: /api/auth (future)                       │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Service Layer: Business Logic                              │ │
│  │ - EmployeeService                                          │ │
│  │ - DepartmentService                                        │ │
│  │ - ValidationService                                        │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Repository Layer: Data Access (JPA)                        │ │
│  │ - EmployeeRepository                                       │ │
│  │ - DepartmentRepository                                     │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                      Data Layer (Databases)                      │
│  ┌──────────────────────┐      ┌─────────────────────────────┐  │
│  │   MySQL Database     │      │   MongoDB (Optional)        │  │
│  │ - Relational Data    │      │ - Document Storage          │  │
│  │ - Employee Table     │      │ - Audit Logs                │  │
│  │ - Department Table   │      │ - Analytics Data            │  │
│  └──────────────────────┘      └─────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Design Patterns Used

1. **MVC Pattern** - Models, Views, Controllers for clear separation
2. **Repository Pattern** - Data access abstraction
3. **Service Layer Pattern** - Business logic encapsulation
4. **DTO Pattern** - Data Transfer Objects for API responses
5. **Factory Pattern** - Component creation in React
6. **Observer Pattern** - React state management and hooks

---

## Technology Stack

### Frontend
- **React 18.3.1** - UI library for building interactive interfaces
- **Material-UI (MUI) 6.0.1** - Professional component library
- **React Router 6.26.1** - Client-side routing
- **Axios 1.7.5** - HTTP client for API calls
- **Chart.js 4.4.4** - Data visualization library
- **Tailwind CSS 3.4.10** - Utility-first CSS framework
- **Jest & React Testing Library** - Testing framework

### Backend
- **Spring Boot 3.3.5** - Java framework for REST APIs
- **Spring Data JPA 3.3.5** - ORM for database operations
- **Hibernate 6.4** - Object-relational mapping
- **MySQL 8.3** - Primary relational database
- **MongoDB** - Document database (optional)
- **JWT 0.12.5** - JSON Web Tokens for authentication
- **Springdoc OpenAPI 2.3.0** - API documentation (Swagger)
- **Lombok 1.18.30** - Boilerplate code reduction
- **JUnit 5** - Testing framework

### DevOps & Deployment
- **Docker** - Containerization
- **Kubernetes** - Container orchestration
- **Jenkins** - CI/CD automation
- **Nginx** - Reverse proxy and load balancer
- **Maven** - Java build automation
- **npm** - Node.js package manager

---

## Directory Structure & File Descriptions

### Root Level Files

```
Employee-Management-Fullstack-App/
├── README.md                    # Main project documentation
├── PROJECT_DOCUMENTATION.md     # This file - In-depth guide
├── CONTRIBUTING.md              # Contribution guidelines
├── CODE_OF_CONDUCT.md           # Community standards
├── LICENSE                      # MIT License
├── .gitignore                   # Git ignore rules
├── .env.example                 # Environment variables template
├── docker-compose.yml           # Docker Compose configuration
├── Jenkinsfile                  # Jenkins CI/CD pipeline
├── openapi.yaml                 # OpenAPI specification
├── Makefile                     # Build automation
└── package.json                 # Project metadata
```

### Frontend Directory (`/frontend`)

```
frontend/
├── package.json                 # Dependencies and scripts
├── Dockerfile                   # Docker image configuration
├── babel.config.js              # Babel transpiler config
├── jest.config.js               # Jest testing config
├── jest.setup.js                # Jest setup
├── tailwind.config.js           # Tailwind CSS config
├── postcss.config.js            # PostCSS config
│
├── public/                      # Static files served directly
│   ├── index.html               # Main HTML file
│   ├── manifest.json            # PWA manifest
│   ├── robots.txt               # SEO robots file
│   ├── sitemap.xml              # SEO sitemap
│   └── favicon.ico              # Website icon
│
├── src/
│   ├── index.js                 # React app entry point
│   ├── App.js                   # Root component
│   ├── App.css                  # Global styles
│   ├── index.css                # Global CSS
│   ├── theme.js                 # MUI theme configuration
│   ├── reportWebVitals.js       # Performance monitoring
│   │
│   ├── components/              # React components
│   │   ├── Dashboard.js         # Dashboard page with charts
│   │   ├── EmployeeList.js      # Employee list and search
│   │   ├── EmployeeForm.js      # Employee add/edit form
│   │   ├── DepartmentList.js    # Department list
│   │   ├── DepartmentForm.js    # Department add/edit form
│   │   ├── Login.js             # Login page
│   │   ├── Register.js          # User registration
│   │   ├── Profile.js           # User profile page
│   │   ├── Navbar.js            # Navigation bar
│   │   ├── Footer.js            # Footer component
│   │   ├── LandingPage.js       # Home/landing page
│   │   ├── NotFoundPage.js      # 404 error page
│   │   ├── ResetPassword.js     # Password reset
│   │   ├── VerifyUsername.js    # Username verification
│   │   └── NewDepartmentForm.js # Create department
│   │
│   ├── services/                # API service calls
│   │   ├── employeeService.js   # Employee API client
│   │   └── departmentService.js # Department API client
│   │
│   └── __tests__/               # Test files
│       ├── Dashboard.spec.js
│       ├── EmployeeList.test.js
│       ├── Login.test.js
│       ├── Register.test.js
│       └── ...more tests
│
└── build/                       # Production build output (generated)
```

#### Frontend Component Details

**Dashboard.js**
- **Purpose:** Display analytics and metrics
- **Features:** Charts showing employee growth, age distribution, total counts
- **Dependencies:** Chart.js, React Chart.js 2
- **API Calls:** Fetches all employees for analytics

**EmployeeList.js**
- **Purpose:** Display all employees in a table/card format
- **Features:** Search, filter, pagination, delete, edit options
- **Dependencies:** MUI Table, Dialog components
- **API Calls:** GET /api/employees, DELETE /api/employees/:id

**EmployeeForm.js**
- **Purpose:** Create and edit employee records
- **Features:** Form validation, department selection, image upload
- **API Calls:** POST /api/employees, PUT /api/employees/:id

**DepartmentList.js & DepartmentForm.js**
- **Purpose:** Manage organizational departments
- **Features:** Add, edit, delete departments

**Login.js & Register.js**
- **Purpose:** User authentication
- **Features:** Form validation, password strength indicator
- **Security:** JWT token storage, secure password handling

#### Frontend Services

**employeeService.js**
```javascript
// API methods for employee operations
- getEmployees()         // GET /api/employees
- getEmployeeById(id)    // GET /api/employees/:id
- createEmployee(data)   // POST /api/employees
- updateEmployee(id, data) // PUT /api/employees/:id
- deleteEmployee(id)     // DELETE /api/employees/:id
```

**departmentService.js**
```javascript
// API methods for department operations
- getDepartments()       // GET /api/departments
- getDepartmentById(id)  // GET /api/departments/:id
- createDepartment(data) // POST /api/departments
- updateDepartment(id, data) // PUT /api/departments/:id
- deleteDepartment(id)   // DELETE /api/departments/:id
```

---

### Backend Directory (`/backend`)

```
backend/
├── pom.xml                      # Maven dependencies
├── Dockerfile                   # Docker image
├── mvnw                         # Maven wrapper (Linux/Mac)
├── mvnw.cmd                     # Maven wrapper (Windows)
├── README.md                    # Backend documentation
│
├── src/main/
│   ├── java/com/example/employeemanagement/
│   │   ├── EmployeeManagementApplication.java
│   │   │                         # Main Spring Boot application
│   │   │
│   │   ├── config/
│   │   │   ├── CorsConfig.java  # CORS configuration
│   │   │   ├── DataInitializer.java # Sample data setup
│   │   │   └── SecurityConfig.java  # Security settings
│   │   │
│   │   ├── controller/
│   │   │   ├── EmployeeController.java  # Employee endpoints
│   │   │   ├── DepartmentController.java # Department endpoints
│   │   │   └── AuthController.java      # Auth endpoints (JWT)
│   │   │
│   │   ├── model/
│   │   │   ├── Employee.java    # Employee entity
│   │   │   ├── Department.java  # Department entity
│   │   │   └── User.java        # User entity (auth)
│   │   │
│   │   ├── repository/
│   │   │   ├── EmployeeRepository.java   # Employee data access
│   │   │   ├── DepartmentRepository.java # Department data access
│   │   │   └── UserRepository.java       # User data access
│   │   │
│   │   ├── service/
│   │   │   ├── EmployeeService.java   # Employee business logic
│   │   │   ├── DepartmentService.java # Department business logic
│   │   │   └── AuthService.java       # Authentication logic
│   │   │
│   │   └── exception/
│   │       ├── ResourceNotFoundException.java
│   │       ├── ValidationException.java
│   │       └── GlobalExceptionHandler.java
│   │
│   └── resources/
│       ├── application.properties     # Configuration file
│       └── data.sql                   # Initial database data
│
├── src/test/java/...           # Unit and integration tests
│   ├── EmployeeManagementApplicationTests.java
│   ├── APITests.java
│   ├── FullAPITests.java
│   └── ...more test files
│
└── target/                      # Compiled output (generated)
```

#### Backend Entity Models

**Employee.java**
```java
@Entity
@Table(name = "employee")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "first_name", nullable = false)
    private String firstName;
    
    @Column(name = "last_name", nullable = false)
    private String lastName;
    
    @Column(name = "email", nullable = false, unique = true)
    private String email;
    
    @Column(name = "phone")
    private String phone;
    
    @Column(name = "address")
    private String address;
    
    @Column(name = "salary")
    private BigDecimal salary;
    
    @Column(name = "join_date")
    private LocalDate joinDate;
    
    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
    
    @Column(name = "created_at")
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
}

// Sample Employee Records:
// 1. John Michael Smith | john.smith@company.com | +1-800-555-0101 | Engineering | $125,000
// 2. Sarah Elizabeth Johnson | sarah.johnson@company.com | +1-800-555-0102 | Product | $118,500
// 3. Michael Robert Chen | michael.chen@company.com | +1-800-555-0103 | Engineering | $132,000
// 4. Jessica Ann Williams | jessica.williams@company.com | +1-800-555-0104 | HR | $95,000
// 5. David James Martinez | david.martinez@company.com | +1-800-555-0105 | Sales | $105,500
```

**Department.java**
```java
@Entity
@Table(name = "department")
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "name", nullable = false, unique = true)
    private String name;
    
    @Column(name = "location")
    private String location;
    
    @Column(name = "manager_name")
    private String managerName;
    
    @Column(name = "number_of_employees")
    private Integer numberOfEmployees;
    
    @Column(name = "budget")
    private BigDecimal budget;
    
    @OneToMany(mappedBy = "department")
    private List<Employee> employees;
    
    @Column(name = "created_at")
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
}

// Sample Department Records:
// 1. Engineering | San Francisco, CA | David Thompson | 45 employees | $12,500,000
// 2. Product Management | San Francisco, CA | Lisa Anderson | 18 employees | $4,200,000
// 3. Sales & Business Development | New York, NY | Robert Wilson | 35 employees | $7,800,000
// 4. Human Resources | Chicago, IL | Margaret Davis | 12 employees | $2,100,000
// 5. Marketing & Communications | Boston, MA | Jennifer Taylor | 22 employees | $3,900,000
// 6. Finance & Accounting | Austin, TX | Thomas Brown | 20 employees | $3,200,000
// 7. Operations | Denver, CO | Christopher Lee | 28 employees | $4,500,000
```

#### Backend Controllers

**EmployeeController.java**
- `GET /api/employees` - Get all employees
- `GET /api/employees/{id}` - Get employee by ID
- `POST /api/employees` - Create new employee
- `PUT /api/employees/{id}` - Update employee
- `DELETE /api/employees/{id}` - Delete employee
- `GET /api/employees/department/{deptId}` - Get employees by department

**DepartmentController.java**
- `GET /api/departments` - Get all departments
- `GET /api/departments/{id}` - Get department by ID
- `POST /api/departments` - Create new department
- `PUT /api/departments/{id}` - Update department
- `DELETE /api/departments/{id}` - Delete department

---

## Frontend Components

### Component Hierarchy

```
App
├── Navbar
│   ├── Navigation Links
│   └── User Menu
├── Routes
│   ├── LandingPage
│   ├── Login
│   ├── Register
│   ├── Dashboard
│   │   └── Charts
│   ├── EmployeeList
│   │   ├── Search Bar
│   │   ├── Filter Options
│   │   ├── Employee Cards/Table
│   │   └── Pagination
│   ├── EmployeeForm
│   │   ├── Form Fields
│   │   └── Department Select
│   ├── DepartmentList
│   │   ├── Search Bar
│   │   └── Department Cards
│   ├── DepartmentForm
│   │   └── Form Fields
│   ├── Profile
│   │   ├── User Info
│   │   ├── Statistics
│   │   └── Settings
│   ├── ResetPassword
│   └── NotFoundPage
└── Footer
    └── Footer Links
```

### Component Communication Flow

```
User Action
    ↓
Component (e.g., EmployeeForm)
    ↓
Service Call (employeeService.js)
    ↓
API Endpoint (Backend)
    ↓
Controller processes request
    ↓
Service layer handles business logic
    ↓
Repository queries database
    ↓
Response returned to Frontend
    ↓
Component updates state
    ↓
UI re-renders
```

---

## Backend Structure

### Request Processing Pipeline

```
HTTP Request
    ↓
Spring DispatcherServlet
    ↓
Controller (e.g., EmployeeController)
    ↓
Validate Request
    ↓
Service Layer (e.g., EmployeeService)
    ↓
Business Logic Processing
    ↓
Repository Layer
    ↓
Database Query (SQL/ORM)
    ↓
Database Returns Data
    ↓
Repository processes result
    ↓
Service processes data
    ↓
Controller formats response
    ↓
HTTP Response (JSON)
```

### Exception Handling

- **GlobalExceptionHandler** - Centralized error handling
- **ResourceNotFoundException** - When requested resource doesn't exist
- **ValidationException** - Input validation errors
- **AuthException** - Authentication/Authorization errors

---

## Database Schema

### Employee Table (MySQL)

```sql
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address VARCHAR(255),
    salary DECIMAL(10,2),
    join_date DATE,
    department_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (department_id) REFERENCES department(id)
);

CREATE INDEX idx_department ON employee(department_id);
CREATE INDEX idx_email ON employee(email);

-- Sample Data
INSERT INTO employee VALUES 
(1, 'John', 'Smith', 'john.smith@company.com', '+1-800-555-0101', '123 Oak Street, San Francisco, CA 94102', 125000.00, '2020-03-15', 1, NOW(), NOW()),
(2, 'Sarah', 'Johnson', 'sarah.johnson@company.com', '+1-800-555-0102', '456 Maple Avenue, San Francisco, CA 94103', 118500.00, '2021-07-20', 2, NOW(), NOW()),
(3, 'Michael', 'Chen', 'michael.chen@company.com', '+1-800-555-0103', '789 Pine Road, San Francisco, CA 94104', 132000.00, '2019-11-10', 1, NOW(), NOW()),
(4, 'Jessica', 'Williams', 'jessica.williams@company.com', '+1-800-555-0104', '321 Elm Street, Chicago, IL 60601', 95000.00, '2022-01-05', 4, NOW(), NOW()),
(5, 'David', 'Martinez', 'david.martinez@company.com', '+1-800-555-0105', '654 Cedar Lane, New York, NY 10001', 105500.00, '2021-05-12', 3, NOW(), NOW()),
(6, 'Emma', 'Thompson', 'emma.thompson@company.com', '+1-800-555-0106', '987 Birch Way, Boston, MA 02101', 98000.00, '2022-09-08', 5, NOW(), NOW()),
(7, 'Robert', 'Garcia', 'robert.garcia@company.com', '+1-800-555-0107', '147 Spruce Court, Austin, TX 78701', 115000.00, '2020-06-22', 6, NOW(), NOW()),
(8, 'Lisa', 'Anderson', 'lisa.anderson@company.com', '+1-800-555-0108', '258 Ash Drive, Denver, CO 80201', 108500.00, '2021-02-14', 7, NOW(), NOW()),
(9, 'James', 'Taylor', 'james.taylor@company.com', '+1-800-555-0109', '369 Willow Street, San Francisco, CA 94105', 128000.00, '2020-08-30', 1, NOW(), NOW()),
(10, 'Angela', 'Brown', 'angela.brown@company.com', '+1-800-555-0110', '741 Chestnut Avenue, New York, NY 10002', 112000.00, '2022-03-18', 3, NOW(), NOW());
```

### Department Table (MySQL)

```sql
CREATE TABLE department (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL UNIQUE,
    location VARCHAR(100),
    manager_name VARCHAR(100),
    number_of_employees INT DEFAULT 0,
    budget DECIMAL(15,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_name ON department(name);

-- Sample Data
INSERT INTO department VALUES 
(1, 'Engineering', 'San Francisco, CA', 'David Thompson', 45, 12500000.00, NOW(), NOW()),
(2, 'Product Management', 'San Francisco, CA', 'Lisa Anderson', 18, 4200000.00, NOW(), NOW()),
(3, 'Sales & Business Development', 'New York, NY', 'Robert Wilson', 35, 7800000.00, NOW(), NOW()),
(4, 'Human Resources', 'Chicago, IL', 'Margaret Davis', 12, 2100000.00, NOW(), NOW()),
(5, 'Marketing & Communications', 'Boston, MA', 'Jennifer Taylor', 22, 3900000.00, NOW(), NOW()),
(6, 'Finance & Accounting', 'Austin, TX', 'Thomas Brown', 20, 3200000.00, NOW(), NOW()),
(7, 'Operations', 'Denver, CO', 'Christopher Lee', 28, 4500000.00, NOW(), NOW());
```

### User Table (Authentication - Future)

```sql
CREATE TABLE user (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(50) DEFAULT 'USER',
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_username ON user(username);

-- Sample Data (passwords hashed with BCrypt)
INSERT INTO user VALUES 
(1, 'john.smith', 'john.smith@company.com', '$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcg7b3XeKeURbnO...', 'ADMIN', TRUE, NOW(), NOW()),
(2, 'sarah.johnson', 'sarah.johnson@company.com', '$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcg7b3XeKeURbnO...', 'USER', TRUE, NOW(), NOW()),
(3, 'michael.chen', 'michael.chen@company.com', '$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcg7b3XeKeURbnO...', 'USER', TRUE, NOW(), NOW()),
(4, 'jessica.williams', 'jessica.williams@company.com', '$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcg7b3XeKeURbnO...', 'HR', TRUE, NOW(), NOW());
```

---

## API Endpoints

### Employee Endpoints

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/api/employees` | Get all employees | - |
| GET | `/api/employees/{id}` | Get employee by ID | - |
| POST | `/api/employees` | Create new employee | Employee JSON |
| PUT | `/api/employees/{id}` | Update employee | Employee JSON |
| DELETE | `/api/employees/{id}` | Delete employee | - |
| GET | `/api/employees/search?q=name` | Search employees | - |

### Department Endpoints

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/api/departments` | Get all departments | - |
| GET | `/api/departments/{id}` | Get department by ID | - |
| POST | `/api/departments` | Create new department | Department JSON |
| PUT | `/api/departments/{id}` | Update department | Department JSON |
| DELETE | `/api/departments/{id}` | Delete department | - |

### Example Request/Response

**Create Employee Request:**
```json
POST /api/employees
{
  "firstName": "Christopher",
  "lastName": "Patterson",
  "email": "christopher.patterson@company.com",
  "phone": "+1-800-555-0111",
  "address": "852 Hickory Lane, San Francisco, CA 94106",
  "salary": 135000,
  "joinDate": "2024-01-15",
  "departmentId": 1
}
```

**Response:**
```json
{
  "id": 11,
  "firstName": "Christopher",
  "lastName": "Patterson",
  "email": "christopher.patterson@company.com",
  "phone": "+1-800-555-0111",
  "address": "852 Hickory Lane, San Francisco, CA 94106",
  "salary": 135000.00,
  "joinDate": "2024-01-15",
  "department": {
    "id": 1,
    "name": "Engineering",
    "location": "San Francisco, CA",
    "managerName": "David Thompson",
    "numberOfEmployees": 46,
    "budget": 12500000.00
  },
  "createdAt": "2024-11-17T10:30:00",
  "updatedAt": "2024-11-17T10:30:00"
}
```

**Get All Employees Response:**
```json
{
  "success": true,
  "message": "Employees retrieved successfully",
  "data": [
    {
      "id": 1,
      "firstName": "John",
      "lastName": "Smith",
      "email": "john.smith@company.com",
      "phone": "+1-800-555-0101",
      "address": "123 Oak Street, San Francisco, CA 94102",
      "salary": 125000.00,
      "joinDate": "2020-03-15",
      "department": {
        "id": 1,
        "name": "Engineering"
      }
    },
    {
      "id": 2,
      "firstName": "Sarah",
      "lastName": "Johnson",
      "email": "sarah.johnson@company.com",
      "phone": "+1-800-555-0102",
      "address": "456 Maple Avenue, San Francisco, CA 94103",
      "salary": 118500.00,
      "joinDate": "2021-07-20",
      "department": {
        "id": 2,
        "name": "Product Management"
      }
    }
  ],
  "totalCount": 10
}
```

**Dashboard Statistics Response:**
```json
{
  "success": true,
  "data": {
    "totalEmployees": 180,
    "averageAge": 34.5,
    "averageSalary": 108750.00,
    "totalSalaryBudget": 19575000.00,
    "employeesByDepartment": {
      "Engineering": 45,
      "Sales": 35,
      "Product Management": 18,
      "Operations": 28,
      "Marketing": 22,
      "Finance": 20,
      "HR": 12
    },
    "employeeGrowth": {
      "2022": 45,
      "2023": 98,
      "2024": 180
    },
    "ageDistribution": {
      "20-25": 15,
      "26-30": 42,
      "31-35": 58,
      "36-40": 42,
      "41-45": 18,
      "45+": 5
    }
  }
}
```

**Create Department Request:**
```json
POST /api/departments
{
  "name": "Customer Success",
  "location": "Seattle, WA",
  "managerName": "Victoria Harris",
  "numberOfEmployees": 25,
  "budget": 3500000.00
}
```

**Get Department by ID Response:**
```json
{
  "id": 8,
  "name": "Customer Success",
  "location": "Seattle, WA",
  "managerName": "Victoria Harris",
  "numberOfEmployees": 25,
  "budget": 3500000.00,
  "employees": [
    {
      "id": 12,
      "firstName": "Rachel",
      "lastName": "Green",
      "email": "rachel.green@company.com",
      "salary": 92000.00
    },
    {
      "id": 13,
      "firstName": "Monica",
      "lastName": "Geller",
      "email": "monica.geller@company.com",
      "salary": 95000.00
    }
  ],
  "createdAt": "2024-11-17T10:30:00",
  "updatedAt": "2024-11-17T10:30:00"
}
```

---

## Deployment & DevOps

### Docker

**Backend Dockerfile** (`backend/Dockerfile`)
- Builds Java Spring Boot application
- Uses OpenJDK 25 base image
- Exposes port 8080
- Runs Spring Boot jar

**Frontend Dockerfile** (`frontend/Dockerfile`)
- Builds React application
- Multi-stage build for optimization
- Serves via Nginx
- Exposes port 3000 (dev) or 80 (prod)

**Docker Compose** (`docker-compose.yml`)
```yaml
- MySQL Service (port 3306)
- MongoDB Service (port 27017)
- Backend Service (port 8080)
- Frontend Service (port 3000)
- Nginx Service (port 80)
```

### Kubernetes

**Configuration Files** (`kubernetes/`)
- `backend-deployment.yaml` - Backend pod configuration
- `frontend-deployment.yaml` - Frontend pod configuration
- `backend-service.yaml` - Backend service exposure
- `frontend-service.yaml` - Frontend service exposure
- `configmap.yaml` - Configuration variables
- Database secrets and volumes

### Jenkins CI/CD

**Jenkinsfile Pipeline Stages:**

1. **Build**
   - Compile backend (Maven)
   - Build frontend (npm)
   - Run unit tests
   - Code quality checks

2. **Test**
   - Integration tests
   - API tests
   - Frontend component tests

3. **Package**
   - Create Docker images
   - Push to Docker Hub
   - Generate artifacts

4. **Deploy**
   - Deploy to Kubernetes cluster
   - Run smoke tests
   - Health checks

---

## Getting Started

### Prerequisites

- **Java 25** - Backend runtime
- **Node.js 18+** - Frontend runtime
- **npm 9+** - Package manager
- **MySQL 8+** - Primary database
- **MongoDB** - Optional document store
- **Docker & Docker Compose** - For containerization
- **Git** - Version control

### Local Setup

#### 1. Clone Repository
```bash
git clone https://github.com/TEJA6777/Employee-Management-Fullstack-App.git
cd Employee-Management-Fullstack-App
```

#### 2. Backend Setup
```bash
cd backend

# Install dependencies
mvn install

# Configure application.properties
# Edit src/main/resources/application.properties with:
cat > src/main/resources/application.properties << EOF
# Server Configuration
server.port=8080
server.servlet.context-path=/api

# MySQL Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/employee_management
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# MongoDB Configuration (Optional)
spring.data.mongodb.uri=mongodb://localhost:27017/employee_management
spring.data.mongodb.auto-index-creation=true

# JWT Configuration
jwt.secret=your-secret-key-change-in-production-min-32-chars
jwt.expiration=86400000

# Logging Configuration
logging.level.root=INFO
logging.level.com.example.employeemanagement=DEBUG
logging.pattern.console=%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n

# CORS Configuration
cors.allowed-origins=http://localhost:3000,http://localhost:8080
cors.allowed-methods=GET,POST,PUT,DELETE,OPTIONS
cors.allowed-headers=*
cors.allow-credentials=true

# Application Info
app.name=Employee Management System
app.version=1.0.0
app.description=Full-Stack Employee Management Application
EOF

# Run backend
mvn spring-boot:run

# Backend available at http://localhost:8080
# API Swagger available at http://localhost:8080/swagger-ui.html
```

#### 3. Frontend Setup
```bash
cd ../frontend

# Install dependencies
npm install

# Create .env file
cat > .env << EOF
REACT_APP_API_URL=http://localhost:8080
REACT_APP_ENV=development
REACT_APP_VERSION=1.0.0
REACT_APP_NAME=Employee Management System
REACT_APP_DEBUG=true
EOF

# Start development server
npm start

# Frontend available at http://localhost:3000
```

#### 4. Database Setup

**MySQL Setup:**
```bash
# Login to MySQL
mysql -u root -p

# Create database and user
CREATE DATABASE employee_management;
CREATE USER 'employee_user'@'localhost' IDENTIFIED BY 'SecurePassword123!';
GRANT ALL PRIVILEGES ON employee_management.* TO 'employee_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

# Verify connection
mysql -u employee_user -p -D employee_management
```

**MongoDB Setup (Optional):**
```bash
# Start MongoDB
mongod

# Create database and collections
mongo

use employee_management

# Create collections
db.createCollection("audit_logs")
db.createCollection("employee_analytics")
db.createCollection("department_budgets")

# Create indexes
db.audit_logs.createIndex({ "timestamp": -1 })
db.audit_logs.createIndex({ "userId": 1 })
db.employee_analytics.createIndex({ "employeeId": 1, "date": -1 })

# Exit
exit
```

### Docker Deployment

```bash
# Build and start all services
docker-compose up --build

# Services Configuration:
# Frontend: http://localhost:3000
# Backend: http://localhost:8080
# Swagger API Docs: http://localhost:8080/swagger-ui.html
# MySQL: localhost:3306 (root:mysql)
# MongoDB: localhost:27017
# Nginx: http://localhost:80

# View logs
docker-compose logs -f frontend
docker-compose logs -f backend
docker-compose logs -f mysql
docker-compose logs -f mongodb

# Stop services
docker-compose down

# Remove all containers and volumes
docker-compose down -v
```

**docker-compose.yml Configuration:**
```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: employee-management-mysql
    environment:
      MYSQL_ROOT_PASSWORD: mysql
      MYSQL_DATABASE: employee_management
      MYSQL_USER: employee_user
      MYSQL_PASSWORD: SecurePassword123!
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - employee-network

  mongodb:
    image: mongo:6.0
    container_name: employee-management-mongodb
    environment:
      MONGO_INITDB_DATABASE: employee_management
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
    networks:
      - employee-network

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: employee-management-backend
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/employee_management
      SPRING_DATASOURCE_USERNAME: employee_user
      SPRING_DATASOURCE_PASSWORD: SecurePassword123!
      SPRING_DATA_MONGODB_URI: mongodb://mongodb:27017/employee_management
      JWT_SECRET: your-secret-key-change-in-production
    ports:
      - "8080:8080"
    depends_on:
      - mysql
      - mongodb
    networks:
      - employee-network

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: employee-management-frontend
    environment:
      REACT_APP_API_URL: http://localhost:8080
      REACT_APP_ENV: production
    ports:
      - "3000:3000"
    depends_on:
      - backend
    networks:
      - employee-network

volumes:
  mysql_data:
  mongodb_data:

networks:
  employee-network:
    driver: bridge
```

### Production Deployment

See `Jenkinsfile` and `kubernetes/` directory for production setup using Kubernetes and Jenkins CI/CD pipeline.

---

## Additional Resources

### API Documentation
- **Interactive Swagger UI:** Navigate to `http://localhost:8080/swagger-ui.html` for interactive API documentation
- **OpenAPI JSON:** `http://localhost:8080/v3/api-docs`
- **OpenAPI YAML:** `http://localhost:8080/v3/api-docs.yaml`

### API Testing Tools
- **Postman Collection:** Available at `/postman_collection.json`
- **Insomnia Collection:** Available at `/insomnia_collection.yaml`
- **cURL Examples:** See API Endpoints section above

### Documentation Links
- **GitHub Repository:** https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform
- **GitHub Issues:** https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform/issues
- **GitHub Discussions:** https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform/discussions
- **Contributing Guide:** See `CONTRIBUTING.md`
- **Code of Conduct:** See `CODE_OF_CONDUCT.md`
- **License:** MIT License (see `LICENSE` file)

### Learning Resources
- **React Documentation:** https://react.dev
- **Spring Boot Documentation:** https://spring.io/projects/spring-boot
- **Material-UI Documentation:** https://mui.com
- **Tailwind CSS Documentation:** https://tailwindcss.com
- **Docker Documentation:** https://docs.docker.com
- **Kubernetes Documentation:** https://kubernetes.io/docs

### Video Tutorials
- **Full-Stack Development:** [YouTube Playlist]
- **React Best Practices:** [YouTube Playlist]
- **Spring Boot Tutorials:** [YouTube Playlist]
- **Docker & Kubernetes:** [YouTube Playlist]

### Community & Support
- **Stack Overflow:** Tag questions with `employee-management` `react` `spring-boot`
- **GitHub Discussions:** Ask questions in the repository discussions
- **Email Support:** Contact maintainers via GitHub profile

---

## Troubleshooting

### Common Issues & Solutions

**Backend won't start**
```
Error: Port 8080 already in use
Solution:
  # Find process using port 8080
  lsof -i :8080
  # Kill the process
  kill -9 <PID>
  # Or change port in application.properties: server.port=8081
```

```
Error: Cannot connect to MySQL
Solution:
  # Verify MySQL is running
  systemctl status mysql
  # Verify credentials
  mysql -u root -p -h localhost
  # Check application.properties
  spring.datasource.url=jdbc:mysql://localhost:3306/employee_management
  spring.datasource.username=root
  spring.datasource.password=your_password
```

```
Error: Package does not exist
Solution:
  # Clean and rebuild
  mvn clean
  mvn install
  # Update dependencies
  mvn dependency:resolve
```

**Frontend build fails**
```
Error: Cannot find module
Solution:
  # Clear npm cache
  npm cache clean --force
  # Remove node_modules and package-lock.json
  rm -rf node_modules package-lock.json
  # Reinstall dependencies
  npm install
```

```
Error: Port 3000 already in use
Solution:
  # Find process using port 3000
  lsof -i :3000
  # Kill the process
  kill -9 <PID>
  # Or run on different port
  npm start -- --port 3001
```

```
Error: API calls failing with CORS errors
Solution:
  # Verify backend CORS config in application.properties
  cors.allowed-origins=http://localhost:3000
  # Restart backend
  # Clear browser cache and cookies
  # Verify API URL in .env file
  REACT_APP_API_URL=http://localhost:8080
```

**API requests returning 401 Unauthorized**
```
Solution:
  # Check JWT token validity
  # Verify token is being sent in headers
  Authorization: Bearer <token>
  # Check token expiration in application.properties
  jwt.expiration=86400000  # 24 hours
```

**Database connection issues**
```
Error: Communications link failure
Solution:
  # Verify MySQL is running
  systemctl start mysql
  # Check database exists
  mysql -u root -p
  SHOW DATABASES;
  USE employee_management;
  SHOW TABLES;
```

**Docker container issues**
```
Error: Container won't start
Solution:
  # View detailed logs
  docker-compose logs backend
  # Rebuild containers
  docker-compose build --no-cache
  # Restart services
  docker-compose restart
  docker-compose down && docker-compose up --build
```

### Performance Optimization Tips

**Database Optimization**
```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_employee_email ON employee(email);
CREATE INDEX idx_employee_department ON employee(department_id);
CREATE INDEX idx_department_location ON department(location);

-- Analyze query performance
EXPLAIN SELECT * FROM employee WHERE email = 'john@example.com';

-- Enable query cache (if not using InnoDB)
SET GLOBAL query_cache_size = 1000000;
```

**Backend Optimization**
```java
// Use pagination for large datasets
@GetMapping("/employees")
public Page<Employee> getEmployees(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "20") int size) {
    return employeeRepository.findAll(PageRequest.of(page, size));
}

// Use caching for frequently accessed data
@Cacheable("departments")
public List<Department> getDepartments() {
    return departmentRepository.findAll();
}

// Use lazy loading for relationships
@ManyToOne(fetch = FetchType.LAZY)
private Department department;
```

**Frontend Optimization**
```javascript
// Use React.memo for expensive components
const EmployeeCard = React.memo(({ employee }) => {
  return <div>{employee.name}</div>;
});

// Use useMemo for expensive calculations
const MemoizedStatistics = useMemo(() => {
  return calculateStatistics(employees);
}, [employees]);

// Use lazy loading for routes
const Dashboard = lazy(() => import('./components/Dashboard'));
```

### Monitoring & Logging

**Enable Debug Logging**
```properties
# application.properties
logging.level.com.example.employeemanagement=DEBUG
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

**View Application Logs**
```bash
# Real-time logs
tail -f logs/application.log

# Search for errors
grep ERROR logs/application.log

# View specific time period
grep "2024-11-17" logs/application.log | head -50
```

**Health Check Endpoint**
```bash
# Access health information
curl http://localhost:8080/actuator/health

# View metrics
curl http://localhost:8080/actuator/metrics

# View specific metric
curl http://localhost:8080/actuator/metrics/jvm.memory.used
```

---

## Testing

### Frontend Testing

**Run all tests**
```bash
npm test
```

**Run specific test file**
```bash
npm test -- EmployeeList.test.js
```

**Example Frontend Tests**

```javascript
// EmployeeList.test.js - Testing Employee List Component
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import EmployeeList from '../components/EmployeeList';
import axios from 'axios';

jest.mock('axios');

describe('EmployeeList Component', () => {
  const mockEmployees = [
    {
      id: 1,
      name: 'John Smith',
      email: 'john@example.com',
      phone: '555-0101',
      position: 'Software Engineer',
      department: { id: 1, name: 'Engineering' }
    },
    {
      id: 2,
      name: 'Sarah Johnson',
      email: 'sarah@example.com',
      phone: '555-0102',
      position: 'Product Manager',
      department: { id: 2, name: 'Product' }
    }
  ];

  beforeEach(() => {
    jest.clearAllMocks();
  });

  test('renders employee list on component mount', async () => {
    axios.get.mockResolvedValue({ data: mockEmployees });

    render(<EmployeeList />);

    await waitFor(() => {
      expect(screen.getByText('John Smith')).toBeInTheDocument();
      expect(screen.getByText('Sarah Johnson')).toBeInTheDocument();
    });
  });

  test('displays loading state while fetching', () => {
    axios.get.mockImplementation(
      () => new Promise(() => {}) // Never resolves
    );

    render(<EmployeeList />);

    expect(screen.getByText(/loading/i)).toBeInTheDocument();
  });

  test('handles API errors gracefully', async () => {
    axios.get.mockRejectedValue(new Error('API Error'));

    render(<EmployeeList />);

    await waitFor(() => {
      expect(screen.getByText(/error/i)).toBeInTheDocument();
    });
  });
});
```

```javascript
// Login.test.js - Testing Login Component
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import Login from '../components/Login';
import axios from 'axios';

jest.mock('axios');

describe('Login Component', () => {
  test('renders login form with email and password fields', () => {
    render(<Login />);

    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/password/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /sign in/i })).toBeInTheDocument();
  });

  test('submits login form with credentials', async () => {
    const mockResponse = { data: { token: 'jwt_token_12345' } };
    axios.post.mockResolvedValue(mockResponse);

    render(<Login />);

    const emailInput = screen.getByLabelText(/email/i);
    const passwordInput = screen.getByLabelText(/password/i);
    const submitButton = screen.getByRole('button', { name: /sign in/i });

    fireEvent.change(emailInput, { target: { value: 'john@example.com' } });
    fireEvent.change(passwordInput, { target: { value: 'password123' } });
    fireEvent.click(submitButton);

    await waitFor(() => {
      expect(axios.post).toHaveBeenCalledWith(
        expect.stringContaining('/api/auth/login'),
        expect.any(Object)
      );
    });
  });

  test('displays error message on failed login', async () => {
    axios.post.mockRejectedValue(new Error('Invalid credentials'));

    render(<Login />);

    const emailInput = screen.getByLabelText(/email/i);
    const passwordInput = screen.getByLabelText(/password/i);
    const submitButton = screen.getByRole('button', { name: /sign in/i });

    fireEvent.change(emailInput, { target: { value: 'invalid@example.com' } });
    fireEvent.change(passwordInput, { target: { value: 'wrong' } });
    fireEvent.click(submitButton);

    await waitFor(() => {
      expect(screen.getByText(/invalid credentials/i)).toBeInTheDocument();
    });
  });
});
```

### Backend Testing

**Run all tests**
```bash
mvn test
```

**Run specific test class**
```bash
mvn test -Dtest=EmployeeServiceTest
```

**Example Backend Tests**

```java
// EmployeeServiceTest.java - Testing Employee Service
package com.example.employeemanagement.service;

import com.example.employeemanagement.entity.Employee;
import com.example.employeemanagement.entity.Department;
import com.example.employeemanagement.repository.EmployeeRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

class EmployeeServiceTest {

    @Mock
    private EmployeeRepository employeeRepository;

    @InjectMocks
    private EmployeeService employeeService;

    private Employee employee;
    private Department department;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);

        department = new Department();
        department.setId(1);
        department.setName("Engineering");

        employee = new Employee();
        employee.setId(1);
        employee.setName("John Smith");
        employee.setEmail("john@example.com");
        employee.setPhone("555-0101");
        employee.setPosition("Software Engineer");
        employee.setSalary(125000.00);
        employee.setDepartment(department);
    }

    @Test
    void testGetEmployeeById() {
        when(employeeRepository.findById(1)).thenReturn(Optional.of(employee));

        Employee result = employeeService.getEmployeeById(1);

        assertNotNull(result);
        assertEquals("John Smith", result.getName());
        assertEquals("john@example.com", result.getEmail());
        verify(employeeRepository, times(1)).findById(1);
    }

    @Test
    void testGetAllEmployees() {
        List<Employee> employees = new ArrayList<>();
        employees.add(employee);

        Employee employee2 = new Employee();
        employee2.setId(2);
        employee2.setName("Sarah Johnson");
        employee2.setEmail("sarah@example.com");
        employees.add(employee2);

        when(employeeRepository.findAll()).thenReturn(employees);

        List<Employee> result = employeeService.getAllEmployees();

        assertEquals(2, result.size());
        assertEquals("John Smith", result.get(0).getName());
        verify(employeeRepository, times(1)).findAll();
    }

    @Test
    void testCreateEmployee() {
        when(employeeRepository.save(employee)).thenReturn(employee);

        Employee result = employeeService.createEmployee(employee);

        assertNotNull(result);
        assertEquals("John Smith", result.getName());
        verify(employeeRepository, times(1)).save(employee);
    }

    @Test
    void testUpdateEmployee() {
        employee.setPosition("Senior Software Engineer");
        employee.setSalary(145000.00);

        when(employeeRepository.save(employee)).thenReturn(employee);

        Employee result = employeeService.updateEmployee(employee);

        assertEquals("Senior Software Engineer", result.getPosition());
        assertEquals(145000.00, result.getSalary());
        verify(employeeRepository, times(1)).save(employee);
    }

    @Test
    void testDeleteEmployee() {
        employeeService.deleteEmployee(1);

        verify(employeeRepository, times(1)).deleteById(1);
    }
}
```

```java
// EmployeeControllerIntegrationTest.java - Testing API Endpoints
package com.example.employeemanagement.controller;

import com.example.employeemanagement.entity.Employee;
import com.example.employeemanagement.entity.Department;
import com.example.employeemanagement.repository.EmployeeRepository;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
class EmployeeControllerIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private EmployeeRepository employeeRepository;

    @Autowired
    private ObjectMapper objectMapper;

    private Employee employee;

    @BeforeEach
    void setUp() {
        employeeRepository.deleteAll();

        Department department = new Department();
        department.setName("Engineering");

        employee = new Employee();
        employee.setName("John Smith");
        employee.setEmail("john@example.com");
        employee.setPhone("555-0101");
        employee.setPosition("Software Engineer");
        employee.setSalary(125000.00);
        employee.setDepartment(department);
    }

    @Test
    void testGetAllEmployees() throws Exception {
        employeeRepository.save(employee);

        mockMvc.perform(get("/api/employees"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].name").value("John Smith"))
            .andExpect(jsonPath("$[0].email").value("john@example.com"));
    }

    @Test
    void testCreateEmployee() throws Exception {
        mockMvc.perform(post("/api/employees")
            .contentType("application/json")
            .content(objectMapper.writeValueAsString(employee)))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John Smith"));
    }

    @Test
    void testUpdateEmployee() throws Exception {
        Employee saved = employeeRepository.save(employee);
        saved.setPosition("Senior Software Engineer");

        mockMvc.perform(put("/api/employees/" + saved.getId())
            .contentType("application/json")
            .content(objectMapper.writeValueAsString(saved)))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.position").value("Senior Software Engineer"));
    }

    @Test
    void testDeleteEmployee() throws Exception {
        Employee saved = employeeRepository.save(employee);

        mockMvc.perform(delete("/api/employees/" + saved.getId()))
            .andExpect(status().isNoContent());
    }
}
```

### Test Coverage

**Generate coverage report**
```bash
# Frontend coverage
npm test -- --coverage

# Backend coverage
mvn clean test jacoco:report
# Report location: backend/target/site/jacoco/index.html
```

**Example Coverage Configuration (pom.xml)**
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.10</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
        <execution>
            <id>jacoco-check</id>
            <goals>
                <goal>check</goal>
            </goals>
            <configuration>
                <rules>
                    <rule>
                        <element>PACKAGE</element>
                        <excludes>
                            <exclude>*Test</exclude>
                        </excludes>
                        <limits>
                            <limit>
                                <counter>LINE</counter>
                                <value>COVEREDRATIO</value>
                                <minimum>0.80</minimum>
                            </limit>
                        </limits>
                    </rule>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
```

---

## Future Enhancements

- [ ] Advanced role-based access control (RBAC)
- [ ] Multi-language support (i18n)
- [ ] Email notifications
- [ ] Salary calculations and payroll
- [ ] Performance reviews system
- [ ] Leave management module
- [ ] Attendance tracking
- [ ] Mobile application (React Native)
- [ ] GraphQL API support
- [ ] Redis caching layer

---

**Last Updated:** November 17, 2025
**Version:** 1.0
**Author:** Kodati Sai Teja
**GitHub:** https://github.com/TEJA6777

For questions or issues, please open an issue on GitHub or contact the maintainers.
