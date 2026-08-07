# Quick Start Guide

Get up and running with the Employee Management Full-Stack Application in 5 minutes!

## Option 1: Docker (Recommended - Easiest)

### Prerequisites
- Docker and Docker Compose installed
- (Optional) Git installed for cloning

### Steps

#### 1. Clone Repository
```bash
git clone https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform.git
cd WorkHub-Modern-Employee-Management-Platform
```

#### 2. Start Services
```bash
docker-compose up -d
```

#### 3. Wait for Services to Start
```bash
# Check status
docker-compose ps

# View logs
docker-compose logs -f
```

#### 4. Access Application
- **Frontend:** http://localhost:3000
- **Backend:** http://localhost:8080
- **API Docs:** http://localhost:8080/swagger-ui.html

#### 5. Login
```
Email: admin@example.com
Password: admin123
```

#### 6. Stop Services
```bash
docker-compose down
```

---

## Option 2: Manual Setup (Advanced)

### Prerequisites
- Java 17+
- Node.js 18+
- MySQL 8.0+
- Git

### Step 1: Clone Repository
```bash
git clone https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform.git
cd WorkHub-Modern-Employee-Management-Platform
```

### Step 2: Setup Database

#### MySQL
```bash
# Connect to MySQL
mysql -u root -p

# Create database and user
CREATE DATABASE employee_management;
CREATE USER 'empuser'@'localhost' IDENTIFIED BY 'emp@123456';
GRANT ALL PRIVILEGES ON employee_management.* TO 'empuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 3: Setup Backend

```bash
cd backend

# Configure database connection
# Edit src/main/resources/application.properties
# Update:
# spring.datasource.username=empuser
# spring.datasource.password=emp@123456

# Build project
mvn clean install

# Run backend
mvn spring-boot:run
```

Backend starts at: **http://localhost:8080**

### Step 4: Setup Frontend

```bash
cd ../frontend

# Install dependencies
npm install

# Create .env file
echo "REACT_APP_API_URL=http://localhost:8080/api" > .env

# Start frontend
npm start
```

Frontend starts at: **http://localhost:3000**

---

## First Time Setup

### 1. Access Application
Open browser to: **http://localhost:3000**

### 2. Login
```
Email: admin@example.com
Password: admin123
```

### 3. Navigate Dashboard
- View employee statistics
- See department information
- Check recent activities

### 4. Add First Employee
1. Click "Employees" in sidebar
2. Click "Add Employee" button
3. Fill in details:
   - Name: John Smith
   - Email: john@example.com
   - Phone: 555-0101
   - Position: Software Engineer
   - Department: Engineering
   - Salary: 125000
4. Click "Save"

### 5. Create Department (if needed)
1. Click "Departments" in sidebar
2. Click "Add Department" button
3. Fill in:
   - Name: Engineering
   - Location: San Francisco, CA
   - Manager: Robert Garcia
   - Budget: 12500000
4. Click "Save"

---

## Common Tasks

### View All Employees
1. Click "Employees" menu
2. See list with pagination
3. Use search to filter

### Edit Employee
1. Go to Employees list
2. Click employee row or edit icon
3. Modify details
4. Click "Save"

### Delete Employee
1. Go to Employees list
2. Click delete icon (trash bin)
3. Confirm deletion

### View Dashboard
1. Click "Dashboard" menu
2. See charts and statistics
3. View department breakdown
4. Check salary ranges

### Search Employees
1. Use search box at top
2. Type name, email, or position
3. Results filter instantly

### Change Password
1. Click profile icon (top right)
2. Click "Settings"
3. Click "Change Password"
4. Enter old and new password
5. Click "Save"

---

## Troubleshooting

### Port Already in Use
```bash
# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# Mac/Linux
lsof -i :3000
kill -9 <PID>
```

### Can't Connect to Backend
```bash
# Check if backend is running
curl http://localhost:8080/actuator/health

# Check API URL in .env
cat frontend/.env

# Verify CORS in backend
# Edit application.properties and check cors settings
```

### Database Connection Error
```bash
# Verify MySQL is running
mysql -u root -p

# Check credentials in application.properties
# Verify database exists
SHOW DATABASES;
```

### Docker Issues
```bash
# View logs
docker-compose logs -f

# Rebuild containers
docker-compose build --no-cache

# Full reset
docker-compose down -v
docker-compose up -d
```

---

## File Structure Overview

```
WorkHub-Modern-Employee-Management-Platform/
├── frontend/              # React application
│   ├── src/
│   │   ├── components/   # UI components
│   │   ├── services/     # API services
│   │   └── App.js        # Main app
│   └── package.json
├── backend/               # Spring Boot application
│   ├── src/
│   │   ├── main/java/    # Source code
│   │   └── resources/    # Configuration
│   └── pom.xml
├── docker-compose.yml     # Services definition
├── README.md              # Project documentation
└── PROJECT_DOCUMENTATION.md # Detailed docs
```

---

## Next Steps

After setup, explore:

1. **[Full Documentation](./PROJECT_DOCUMENTATION.md)** - Complete guide
2. **[API Reference](./API_REFERENCE.md)** - API endpoints
3. **[Features Guide](./FEATURES.md)** - All features
4. **[Security Guide](./SECURITY.md)** - Security best practices
5. **[FAQ](./FAQ.md)** - Common questions
6. **[Deployment Guide](./DEPLOYMENT_GUIDE.md)** - Production deployment

---

## Key Commands

### Docker Commands
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# Restart specific service
docker-compose restart backend

# Build images
docker-compose build --no-cache
```

### Backend Commands
```bash
# Build
mvn clean install

# Run
mvn spring-boot:run

# Run tests
mvn test

# Generate API docs
mvn springdoc-openapi:generate
```

### Frontend Commands
```bash
# Install dependencies
npm install

# Start development
npm start

# Run tests
npm test

# Build for production
npm run build

# Deploy
npm run deploy
```

---

## Environment Variables

### Backend (.env or application.properties)
```properties
SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/employee_management
SPRING_DATASOURCE_USERNAME=empuser
SPRING_DATASOURCE_PASSWORD=emp@123456
JWT_SECRET=your-secret-key-min-256-characters
SERVER_PORT=8080
```

### Frontend (.env)
```
REACT_APP_API_URL=http://localhost:8080/api
REACT_APP_ENV=development
PORT=3000
```

---

## Database Sample Data

The application includes sample data:

### Sample Employees (10 records)
- John Smith - Software Engineer - $125,000
- Sarah Johnson - Product Manager - $118,500
- Michael Chen - Data Scientist - $132,000
- Jessica Williams - UX Designer - $95,000
- David Martinez - DevOps Engineer - $105,500
- Emma Thompson - QA Engineer - $98,000
- Robert Garcia - Senior Engineer - $115,000
- Lisa Anderson - Business Analyst - $108,500
- James Taylor - Architect - $128,000
- Angela Brown - Frontend Engineer - $112,000

### Sample Departments (7 records)
- Engineering (45 employees, $12.5M)
- Product (18 employees, $4.2M)
- Sales (35 employees, $7.8M)
- HR (12 employees, $2.1M)
- Marketing (22 employees, $3.9M)
- Finance (20 employees, $3.2M)
- Operations (28 employees, $4.5M)

---

## Performance Tips

1. **Use Docker** for consistent environment
2. **Enable Caching** for faster queries
3. **Add Indexes** to frequently searched columns
4. **Use Pagination** for large datasets
5. **Monitor Logs** for performance issues
6. **Scale Horizontally** by adding more containers

---

## Security Reminders

⚠️ **Important for Production:**
1. Change default admin password immediately
2. Use strong JWT secret (256+ characters)
3. Enable SSL/TLS encryption
4. Update database credentials
5. Configure CORS properly
6. Enable audit logging
7. Set up backups
8. Monitor for suspicious activity

---

## Support & Help

- 📖 **Documentation:** See [README.md](./README.md)
- 🔍 **API Docs:** http://localhost:8080/swagger-ui.html
- 🐛 **Issues:** https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform/issues
- 💬 **Discussions:** https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform/discussions
- 📧 **Email:** saitejakodati6777@gmail.com

---

**Ready to get started?** Choose Option 1 (Docker) for the fastest setup! ⚡

**Last Updated:** November 17, 2025  
**Maintainer:** Kodati Sai Teja  
**GitHub:** https://github.com/TEJA6777
