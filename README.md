# WorkHub - Modern Employee Management Platform

A comprehensive full-stack employee management solution built with modern technologies. WorkHub provides enterprise-grade features for managing employees, departments, analytics, and user authentication with a sleek, responsive interface.

**Built by:** Ch Siddhartha | **GitHub:** [https://github.com/mr-siddhu-0201) | **Email:** siddhusiddhartha996@gmail.com
---

## 🎯 Quick Overview

WorkHub is a production-ready employee management platform that combines:
- **React 18** frontend with Material-UI components
- **Spring Boot 3** backend with comprehensive REST APIs
- **MySQL/MongoDB** database support
- **Docker & Kubernetes** deployment ready
- **Modern authentication** with JWT tokens
- **Real-time analytics** and dashboards
- **Responsive design** for all devices

---

## 📋 Table of Contents

1. [Features](#-features)
2. [Tech Stack](#-tech-stack)
3. [Quick Start](#-quick-start)
4. [Installation](#-installation)
5. [Configuration](#-configuration)
6. [Running the Application](#-running-the-application)
7. [API Documentation](#-api-documentation)
8. [Project Structure](#-project-structure)
9. [Deployment](#-deployment)
10. [Contributing](#-contributing)
11. [License](#-license)
12. [Support](#-support)

---

## ✨ Features

### Core Functionality
- ✅ **Employee Management** - CRUD operations for employee records
- ✅ **Department Management** - Organize employees by departments
- ✅ **User Authentication** - Secure JWT-based authentication
- ✅ **Role-Based Access Control** - Admin, Manager, and Employee roles
- ✅ **Real-time Dashboard** - Analytics and employee statistics
- ✅ **Advanced Search** - Filter and search employees by various criteria
- ✅ **Data Export** - Export employee data (ready for implementation)

### Technical Features
- ✅ **REST API** - Comprehensive API with Swagger documentation
- ✅ **Responsive Design** - Works on desktop, tablet, and mobile
- ✅ **Error Handling** - Comprehensive error messages and validation
- ✅ **Security** - Password encryption, CORS protection, input validation
- ✅ **Containerization** - Docker support for easy deployment
- ✅ **CI/CD Ready** - GitHub Actions, Jenkins support
- ✅ **Testing** - Unit tests for both frontend and backend

---

## 🛠️ Tech Stack

### Frontend
| Technology | Version | Purpose |
|-----------|---------|---------|
| React | 18.3.1 | UI framework |
| Material-UI | 6.0.1 | Component library |
| React Router | 6.26.1 | Navigation |
| Axios | 1.7.5 | HTTP client |
| Chart.js | 4.4.4 | Data visualization |
| Tailwind CSS | 3.4.10 | Utility CSS |
| Jest | 27.5.1 | Testing |

### Backend
| Technology | Version | Purpose |
|-----------|---------|---------|
| Java | 25 | Language |
| Spring Boot | 3.3.5 | Framework |
| Spring Data JPA | - | ORM |
| MySQL | 8.3 | Database |
| JUnit 5 | - | Testing |
| Swagger/OpenAPI | 2.3.0 | API docs |

### DevOps
| Technology | Purpose |
|-----------|---------|
| Docker | Containerization |
| Kubernetes | Orchestration |
| Jenkins | CI/CD |
| GitHub Actions | Automation |
| Nginx | Reverse proxy |

---

## 🚀 Quick Start

### Option 1: Docker (Easiest - 5 minutes)

```bash
# Clone the repository
git clone https://github.com/mr-siddhu-0201/Employee-Management-Platform.git
cd Employee-Management-Platform

# Start with Docker Compose
docker-compose up -d

# Access the application
# Frontend: http://localhost:3000
# Backend: http://localhost:8080
# API Docs: http://localhost:8080/swagger-ui.html
```

### Option 2: Manual Setup

```bash
# Clone repository
git clone https://github.com/mr-siddhu-0201/Employee-Management-Platform.git
cd Employee-Management-Platform

# Backend setup
cd backend
mvn clean install
mvn spring-boot:run

# Frontend setup (in new terminal)
cd frontend
npm install
npm start
```

---

## 📦 Installation

### Prerequisites
- **Docker & Docker Compose** (for containerized deployment) OR
- **Java 17+** (for backend)
- **Node.js 18+** (for frontend)
- **MySQL 8.0+** (if not using Docker)
- **Git**

### Step 1: Clone Repository

```bash
git clone https://github.com/mr-siddhu-0201/Employee-Management-Platform.git
cd WorkHub-Modern-Employee-Management-Platform
```

### Step 2: Choose Your Setup Method

#### Docker Setup (Recommended)
```bash
docker-compose up -d
```

#### Manual Backend Setup
```bash
cd backend

# Build the project
mvn clean install

# Configure database (edit src/main/resources/application.properties)
# spring.datasource.url=jdbc:mysql://localhost:3306/employee_management
# spring.datasource.username=root
# spring.datasource.password=yourpassword

# Run the server
mvn spring-boot:run
```

#### Manual Frontend Setup
```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm start
```

---

## ⚙️ Configuration

### Backend Configuration

Edit `backend/src/main/resources/application.properties`:

```properties
# Server
server.port=8080
server.servlet.context-path=/api

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/employee_management
spring.datasource.username=root
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false

# JWT
jwt.secret=your_secret_key_here
jwt.expiration=86400000

# Swagger
springdoc.api-docs.path=/v3/api-docs
springdoc.swagger-ui.path=/swagger-ui.html
```

### Frontend Configuration

Environment variables are managed in `frontend/src/config/apiConfig.js`:

```javascript
// Development: Uses http://localhost:8080
// Production: Uses your deployed backend URL
```

---

## ▶️ Running the Application

### Using Docker Compose

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Local Development

**Terminal 1 - Backend:**
```bash
cd backend
mvn spring-boot:run
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm start
```

### Access Points

| Service | URL | Username | Password |
|---------|-----|----------|----------|
| Frontend | http://localhost:3000 | admin | admin123 |
| Backend API | http://localhost:8080 | - | - |
| Swagger UI | http://localhost:8080/swagger-ui.html | - | - |
| Database | localhost:3306 | root | root123 |

---

## 📚 API Documentation

### Access Swagger UI
Navigate to: **http://localhost:8080/swagger-ui.html**

### Sample Endpoints

**Authentication**
```bash
POST /api/authenticate
POST /api/register
POST /api/verify-username/{username}
```

**Employees**
```bash
GET    /api/employees
POST   /api/employees
GET    /api/employees/{id}
PUT    /api/employees/{id}
DELETE /api/employees/{id}
```

**Departments**
```bash
GET    /api/departments
POST   /api/departments
GET    /api/departments/{id}
PUT    /api/departments/{id}
DELETE /api/departments/{id}
```

For complete API reference, see [API_REFERENCE.md](./API_REFERENCE.md)

---

## 📂 Project Structure

```
WorkHub-Modern-Employee-Management-Platform/
├── frontend/                          # React application
│   ├── src/
│   │   ├── components/               # 15 React components
│   │   ├── services/                 # API services
│   │   ├── config/                   # Environment configuration
│   │   └── App.js                    # Main app
│   ├── package.json
│   └── Dockerfile
│
├── backend/                           # Spring Boot application
│   ├── src/
│   │   ├── main/java/               # Java source code
│   │   └── resources/               # Configuration files
│   ├── pom.xml
│   └── Dockerfile
│
├── kubernetes/                        # K8s manifests
│   ├── backend-deployment.yaml
│   ├── frontend-deployment.yaml
│   └── backend-service.yaml
│
├── docker-compose.yml                 # Docker Compose config
├── netlify.toml                       # Netlify configuration
└── README.md                          # This file
```

---

## 🌐 Deployment

### Deploy to Netlify (Frontend)

1. Connect your GitHub repository to Netlify
2. Set build command: `cd frontend && npm ci && npm run build`
3. Set publish directory: `frontend/build`
4. Deploy!

See [NETLIFY_HEROKU_DEPLOYMENT.md](./NETLIFY_HEROKU_DEPLOYMENT.md) for detailed instructions.

### Deploy to Heroku (Backend)

```bash
heroku create your-app-name
git push heroku master
```

### Deploy to AWS/Azure/GCP

See [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md) for comprehensive cloud deployment options.

### Deploy to Kubernetes

```bash
kubectl apply -f kubernetes/
```

---

## 🧪 Testing

### Frontend Tests
```bash
cd frontend
npm test                      # Run tests
npm run test:coverage        # Generate coverage report
```

### Backend Tests
```bash
cd backend
mvn test                     # Run JUnit tests
mvn test jacoco:report       # Generate coverage report
```

---

## 📖 Documentation

- **[Quick Start Guide](./QUICKSTART.md)** - Get started in 5 minutes
- **[API Reference](./API_REFERENCE.md)** - Complete API documentation
- **[Deployment Guide](./DEPLOYMENT_GUIDE.md)** - Production deployment
- **[Security Guide](./SECURITY.md)** - Security best practices
- **[FAQ](./FAQ.md)** - Frequently asked questions
- **[Features](./FEATURES.md)** - Detailed feature list
- **[Troubleshooting](./PROJECT_DOCUMENTATION.md#troubleshooting)** - Common issues and solutions

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

**Steps to contribute:**
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add your feature"`
4. Push to branch: `git push origin feature/your-feature`
5. Submit a Pull Request

---


## 📞 Support & Contact

### Get Help

- 🐛 **Report Issues:** [GitHub Issues](https://github.com/mr-siddhu-0201/Employee-Management-Platform)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/mr-siddhu-0201/Employee-Management-Platform/discussions)
- ❓ **FAQ:** [Check our FAQ](./FAQ.md)
- 📚 **Documentation:** [View all docs](./DOCUMENTATION_INDEX.md)

### Contact Information

- **Author:** Ch Siddhartha
- **Email:** siddhusiddhartha996@gmail.com
- **GitHub:** [@Siddhu](https://github.com/mr-siddhu-0201/)
- **Repository:** [Employee Management Platform](https://github.com/mr-siddhu-0201/Employee-Management-Platform)

---

## 🎓 Learning Resources

This project demonstrates:
- Full-stack web application development
- Modern React patterns and hooks
- Spring Boot microservices
- RESTful API design
- Authentication & authorization
- Docker containerization
- Kubernetes orchestration
- CI/CD pipelines
- Production-ready code practices

---

## 🙏 Acknowledgments

- **Material-UI** for beautiful React components
- **Spring Framework** team for excellent documentation
- **React** community for amazing tools and libraries
- **Docker & Kubernetes** for containerization
- All contributors and users of this project

