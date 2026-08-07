# Deployment on Netlify & Heroku Guide

Complete guide to deploy the Employee Management Full-Stack Application on Netlify (Frontend) and Heroku (Backend).

## Table of Contents

1. [Netlify Deployment (Frontend)](#netlify-deployment-frontend)
2. [Heroku Deployment (Backend)](#heroku-deployment-backend)
3. [Environment Configuration](#environment-configuration)
4. [Custom Domain Setup](#custom-domain-setup)
5. [Database on Cloud](#database-on-cloud)
6. [Monitoring & Logging](#monitoring--logging)
7. [CI/CD Pipeline](#cicd-pipeline)
8. [Troubleshooting](#troubleshooting)

---

## Netlify Deployment (Frontend)

### Prerequisites

- Netlify account (free at https://netlify.com)
- GitHub account with forked repository
- Node.js 18+

### Step 1: Connect GitHub Repository

1. Go to https://netlify.com and sign in
2. Click "New site from Git"
3. Select "GitHub"
4. Authorize Netlify to access your GitHub account
5. Select repository: `WorkHub-Modern-Employee-Management-Platform`
6. Click "Connect repository"

### Step 2: Configure Build Settings

1. **Build command:**
```bash
cd frontend && npm install && npm run build
```

2. **Publish directory:**
```
frontend/build
```

3. **Environment variables:**
Click "Advanced" and add:
```
REACT_APP_API_URL = https://your-heroku-backend.herokuapp.com/api
NODE_ENV = production
CI = false
```

### Step 3: Deploy

1. Click "Deploy site"
2. Wait for build to complete
3. Your site will be available at: `https://your-site-name.netlify.app`

### Complete Build Settings Example

```yaml
Build Settings:
  Build command: cd frontend && npm install && npm run build
  Publish directory: frontend/build
  Functions directory: (leave empty)
  Base directory: (leave empty)

Environment:
  REACT_APP_API_URL: https://your-backend.herokuapp.com/api
  NODE_ENV: production
  CI: false

Build & deploy:
  Branches and deploy contexts:
    Production branch: main
    Deploy previews: All
    Branch deploys: All
```

### Step 4: Configure Netlify TOML (Optional)

Create `netlify.toml` in root directory:

```toml
[build]
  command = "cd frontend && npm install && npm run build"
  publish = "frontend/build"
  functions = "functions"

[build.environment]
  REACT_APP_API_URL = "https://your-backend.herokuapp.com/api"
  NODE_ENV = "production"
  CI = "false"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[context.production.environment]
  REACT_APP_API_URL = "https://your-backend.herokuapp.com/api"

[context.deploy-preview.environment]
  REACT_APP_API_URL = "https://your-backend-staging.herokuapp.com/api"

[[headers]]
  for = "/*"
  [headers.values]
    X-Content-Type-Options = "nosniff"
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    Strict-Transport-Security = "max-age=31536000; includeSubDomains"
```

### Frontend Deployment Complete!

Your React app is now live at: **https://your-site-name.netlify.app**

---

## Heroku Deployment (Backend)

### Prerequisites

- Heroku account (free at https://heroku.com)
- Heroku CLI installed (https://devcenter.heroku.com/articles/heroku-cli)
- GitHub repository with backend code
- Java 17+

### Step 1: Install Heroku CLI

```bash
# Windows (using npm)
npm install -g heroku

# Or download from https://devcenter.heroku.com/articles/heroku-cli

# Verify installation
heroku --version
```

### Step 2: Login to Heroku

```bash
heroku login
# Opens browser to login
# Authorize and return to terminal
```

### Step 3: Create Heroku App

```bash
# Create new app
heroku create your-app-name

# Or create with specific region
heroku create your-app-name --region eu

# Verify app created
heroku apps
```

### Step 4: Add PostgreSQL Database

```bash
# Add free Postgres database
heroku addons:create heroku-postgresql:hobby-dev

# View database URL
heroku config | grep DATABASE_URL
```

### Step 5: Configure Environment Variables

```bash
# Set production environment
heroku config:set SPRING_PROFILES_ACTIVE=prod

# Set JWT secret
heroku config:set JWT_SECRET=your-super-secret-key-min-256-characters

# Set database URL (automatic with addon)
heroku config:set SPRING_DATASOURCE_URL=postgresql://...

# Set allowed origins for CORS
heroku config:set CORS_ALLOWED_ORIGINS=https://your-site-name.netlify.app

# View all config variables
heroku config
```

### Step 6: Create Procfile

Create `Procfile` in backend root:

```procfile
web: java -Dserver.port=$PORT $JAVA_OPTS -jar target/*.jar
```

### Step 7: Create system.properties

Create `system.properties` in backend root:

```properties
java.runtime.version=17
```

### Step 8: Prepare for Deployment

```bash
cd backend

# Build Maven project
mvn clean package -DskipTests

# Verify JAR created
ls target/*.jar
```

### Step 9: Deploy to Heroku

```bash
# Add Heroku as git remote
heroku git:remote -a your-app-name

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

### Step 10: Verify Deployment

```bash
# Check app status
heroku ps

# Open app in browser
heroku open

# Check health endpoint
curl https://your-app-name.herokuapp.com/actuator/health
```

### Backend Deployment Complete!

Your Spring Boot app is now live at: **https://your-app-name.herokuapp.com**

---

## Environment Configuration

### Update Frontend API URL

After backend deployment, update frontend:

```bash
# In Netlify dashboard:
1. Go to Site settings > Build & deploy > Environment
2. Update REACT_APP_API_URL to: https://your-app-name.herokuapp.com/api
3. Trigger new deploy

# Or via netlify.toml:
[build.environment]
  REACT_APP_API_URL = "https://your-app-name.herokuapp.com/api"
```

### Update Backend CORS Settings

```properties
# application.properties
cors.allowed-origins=https://your-site-name.netlify.app,https://localhost:3000
cors.allowed-methods=GET,POST,PUT,DELETE,OPTIONS,PATCH
cors.allowed-headers=Content-Type,Authorization
cors.max-age=3600
```

### Heroku Backend Environment Variables

```bash
# Production configuration
heroku config:set SPRING_DATASOURCE_URL=postgresql://user:pass@host:port/dbname
heroku config:set SPRING_DATASOURCE_USERNAME=postgres
heroku config:set SPRING_DATASOURCE_PASSWORD=password
heroku config:set SPRING_JPA_HIBERNATE_DDL_AUTO=update
heroku config:set SPRING_JPA_SHOW_SQL=false
heroku config:set JWT_EXPIRATION=86400000
heroku config:set JWT_SECRET=your-secret-key
heroku config:set LOGGING_LEVEL_ROOT=INFO
heroku config:set LOGGING_LEVEL_COM_EXAMPLE_EMPLOYEEMANAGEMENT=DEBUG
```

---

## Custom Domain Setup

### Netlify Custom Domain

1. Go to Site settings > Domain management
2. Click "Add domain alias"
3. Enter your domain: `app.yourdomain.com`
4. Netlify provides nameservers
5. Update your domain registrar to point to Netlify nameservers
6. SSL certificate auto-provisioned (free)

### Heroku Custom Domain

```bash
# Add custom domain to Heroku app
heroku domains:add api.yourdomain.com

# Get DNS target
heroku domains

# Add CNAME record to your domain registrar
# CNAME: api.yourdomain.com -> your-app-name.herokuapp.com

# Verify domain
heroku domains

# SSL auto-provisioned
```

### Complete Configuration

**Frontend:** `app.yourdomain.com` → Netlify  
**Backend:** `api.yourdomain.com` → Heroku  
**CORS:** Configure backend to allow `https://app.yourdomain.com`

---

## Database on Cloud

### Heroku PostgreSQL (Default)

Already configured! Access via:

```bash
# Connect to database
heroku pg:psql

# View database info
heroku pg:info

# Backups
heroku pg:backups

# Create backup
heroku pg:backups:capture

# Download backup
heroku pg:backups:download
```

### AWS RDS Alternative

```bash
# Create RDS instance
aws rds create-db-instance \
  --db-instance-identifier employee-management \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username postgres \
  --master-user-password password123

# Get endpoint
aws rds describe-db-instances \
  --db-instance-identifier employee-management
```

### MongoDB Atlas (Optional)

```bash
# Create MongoDB cluster at https://www.mongodb.com/cloud/atlas
# Get connection string
# Add to Heroku config

heroku config:set SPRING_DATA_MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/employee_management
```

---

## Monitoring & Logging

### Heroku Logs

```bash
# View recent logs
heroku logs --num 50

# Real-time logs
heroku logs --tail

# Filter by dyno
heroku logs --ps web

# Search logs
heroku logs | grep ERROR
```

### Add-ons for Monitoring

```bash
# Papertrail (log management)
heroku addons:create papertrail:choklad

# New Relic (performance monitoring)
heroku addons:create newrelic:wayne

# SendGrid (email)
heroku addons:create sendgrid:starter

# View add-ons
heroku addons
```

### Netlify Analytics

1. Site settings > Analytics
2. Enable to track:
   - Page views
   - Unique visitors
   - Bounce rate
   - Referrers

---

## CI/CD Pipeline

### GitHub Actions for Netlify

Create `.github/workflows/netlify-deploy.yml`:

```yaml
name: Deploy to Netlify

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      
      - name: Build
        env:
          REACT_APP_API_URL: ${{ secrets.REACT_APP_API_URL }}
        run: cd frontend && npm install && npm run build
      
      - name: Deploy to Netlify
        uses: netlify/actions/cli@master
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
        with:
          args: deploy --prod --dir=frontend/build
```

### GitHub Actions for Heroku

Create `.github/workflows/heroku-deploy.yml`:

```yaml
name: Deploy to Heroku

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      
      - name: Deploy to Heroku
        env:
          HEROKU_API_KEY: ${{ secrets.HEROKU_API_KEY }}
          HEROKU_APP_NAME: ${{ secrets.HEROKU_APP_NAME }}
        run: |
          git remote add heroku https://git.heroku.com/$HEROKU_APP_NAME.git
          git push heroku main
```

### Add GitHub Secrets

1. Go to GitHub repo Settings > Secrets
2. Add:
   - `NETLIFY_AUTH_TOKEN` - from Netlify
   - `NETLIFY_SITE_ID` - from Netlify
   - `HEROKU_API_KEY` - from Heroku
   - `HEROKU_APP_NAME` - your Heroku app name
   - `REACT_APP_API_URL` - backend API URL

---

## Troubleshooting

### Netlify Build Fails

```bash
# Check build log in Netlify dashboard
# Common fixes:
1. Clear cache: Site settings > Clear cache and retry deploy
2. Update Node version: Set NODE_VERSION env var
3. Check .gitignore: Ensure node_modules not in git
```

### Heroku Deployment Fails

```bash
# Check if Procfile exists
ls Procfile

# Check if system.properties exists
ls system.properties

# View deployment logs
heroku logs --tail

# Common fixes:
1. mvn clean package -DskipTests
2. Push again: git push heroku main
3. Check Java version: java -version should be 17+
```

### Frontend Can't Connect to Backend

```bash
# Check CORS configuration
curl -H "Origin: https://your-site-name.netlify.app" \
     http://your-app-name.herokuapp.com/api/employees

# Update Heroku CORS
heroku config:set CORS_ALLOWED_ORIGINS=https://your-site-name.netlify.app

# Restart app
heroku restart
```

### Database Connection Error

```bash
# Check database URL
heroku config | grep DATABASE

# Test connection
heroku pg:psql

# Migrate database
heroku run "mvn flyway:migrate"
```

### Out of Memory Error

```bash
# Upgrade Heroku dyno
heroku ps:type web=standard-1x

# Or scale up
heroku ps:scale web=2
```

---

## Costs & Pricing

### Netlify
- **Free Tier:** Perfect for development
  - 100 GB bandwidth/month
  - Unlimited sites
  - Auto SSL
  - Built-in CI/CD

- **Pro:** $19/month
  - 1 TB bandwidth
  - Priority support

### Heroku
- **Free Tier:** (Discontinued - use paid tier)
  
- **Eco:** $5/month (shared dyno)
- **Basic:** $7/month (standard dyno)
- **Production:** $50+/month (performance dyno)

- **Database:**
  - Free tier: Not available
  - Postgres: Starts at $9/month
  - MongoDB Atlas: Free tier available

### Cost-Saving Tips
1. Use Netlify free tier for frontend
2. Use Heroku free tier alternatives (Railway, Render, Replit)
3. Use MongoDB Atlas free tier for database
4. Use GitHub free tier for CI/CD
5. Monitor Heroku usage to avoid overage charges

---

## Deployment Checklist

- [ ] Fork repository on GitHub
- [ ] Create Netlify account and connect GitHub
- [ ] Configure Netlify build settings
- [ ] Create Heroku account
- [ ] Create Heroku app
- [ ] Add PostgreSQL database
- [ ] Configure Heroku environment variables
- [ ] Add Procfile and system.properties
- [ ] Deploy backend to Heroku
- [ ] Update frontend API URL
- [ ] Deploy frontend to Netlify
- [ ] Test API connectivity
- [ ] Configure custom domains
- [ ] Set up monitoring
- [ ] Configure CI/CD pipelines
- [ ] Test production deployment
- [ ] Monitor logs and performance

---

## Quick Commands Reference

```bash
# Netlify
netlify deploy --prod          # Manual deploy
netlify open                   # Open site in browser
netlify env:list              # List environment variables

# Heroku
heroku create my-app          # Create new app
heroku deploy                 # Deploy from git
heroku logs --tail            # View logs
heroku open                   # Open app in browser
heroku config                 # List config variables
heroku ps                     # View dyno status
heroku restart                # Restart app
heroku run "command"          # Run command on dyno
```

---

**Last Updated:** November 17, 2025  
**Maintainer:** Kodati Sai Teja  
**GitHub:** https://github.com/TEJA6777

For more help, visit:
- Netlify Docs: https://docs.netlify.com
- Heroku Docs: https://devcenter.heroku.com
