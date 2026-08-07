# Advanced Deployment Guide

## Table of Contents

1. [AWS Deployment](#aws-deployment)
2. [Azure Deployment](#azure-deployment)
3. [Google Cloud Deployment](#google-cloud-deployment)
4. [Kubernetes Deployment](#kubernetes-deployment)
5. [CI/CD Pipelines](#cicd-pipelines)
6. [Monitoring & Logging](#monitoring--logging)
7. [Performance Tuning](#performance-tuning)

---

## AWS Deployment

### Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    Route 53 (DNS)                        │
└──────────────────────────┬──────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────┐
│           CloudFront (CDN) + WAF                         │
└──────────────────────────┬──────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
    ┌───▼────┐        ┌───▼────┐       ┌───▼────┐
    │  ALB   │        │  ALB   │       │  ALB   │
    │ (us-e1)│        │ (us-w2)│       │ (eu-w1)│
    └───┬────┘        └───┬────┘       └───┬────┘
        │                  │                │
    ┌───▼──────────┐  ┌───▼──────────┐  ┌───▼──────────┐
    │ ECS Cluster  │  │ ECS Cluster  │  │ ECS Cluster  │
    │ (Frontend)   │  │ (Backend)    │  │ (Multi-AZ)   │
    │ (Backend)    │  │              │  │              │
    └───┬──────────┘  └───┬──────────┘  └───┬──────────┘
        │                  │                │
    ┌───▴──────────────────┴────────────────┴───┐
    │         RDS Multi-AZ (MySQL 8.3)          │
    │      Automated backups, encryption        │
    └────────────────────────────────────────────┘
        │
    ┌───▴──────────────────────────────────────┐
    │         ElastiCache (Redis)              │
    │      Session caching, data caching       │
    └──────────────────────────────────────────┘
        │
    ┌───▴──────────────────────────────────────┐
    │          S3 (File Storage)               │
    │  Logs, backups, static assets            │
    └──────────────────────────────────────────┘
```

### Terraform Configuration for AWS

```hcl
# terraform/aws/main.tf

# VPC Configuration
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "employee-management-vpc"
  }
}

# Public Subnets (2 AZs)
resource "aws_subnet" "public" {
  count                   = 2
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.${count.index + 1}.0/24"
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true

  tags = {
    Name = "public-subnet-${count.index + 1}"
  }
}

# Private Subnets for RDS (2 AZs)
resource "aws_subnet" "private" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 10}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]

  tags = {
    Name = "private-subnet-${count.index + 1}"
  }
}

# RDS MySQL Instance
resource "aws_db_instance" "mysql" {
  identifier            = "employee-management-db"
  engine                = "mysql"
  engine_version        = "8.3"
  instance_class        = "db.t3.medium"
  allocated_storage     = 100
  max_allocated_storage = 1000
  
  db_name  = "employee_management"
  username = "admin"
  password = random_password.db_password.result
  
  multi_az               = true
  publicly_accessible    = false
  skip_final_snapshot    = false
  final_snapshot_identifier_prefix = "employee-management-final"
  
  backup_retention_period = 30
  backup_window          = "03:00-04:00"
  maintenance_window     = "sun:04:00-sun:05:00"
  
  storage_encrypted = true
  kms_key_id       = aws_kms_key.rds.arn
  
  vpc_security_group_ids = [aws_security_group.rds.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name
  
  tags = {
    Name = "employee-management-db"
  }
}

# ElastiCache Redis
resource "aws_elasticache_cluster" "redis" {
  cluster_id           = "employee-management-redis"
  engine               = "redis"
  engine_version       = "7.0"
  node_type            = "cache.t3.micro"
  num_cache_nodes      = 2
  parameter_group_name = aws_elasticache_parameter_group.redis.name
  
  subnet_group_name       = aws_elasticache_subnet_group.main.name
  security_group_ids      = [aws_security_group.redis.id]
  
  automatic_failover_enabled = true
  at_rest_encryption_enabled = true
  transit_encryption_enabled = true
  auth_token                = random_password.redis_auth_token.result
  
  tags = {
    Name = "employee-management-redis"
  }
}

# ECS Cluster
resource "aws_ecs_cluster" "main" {
  name = "employee-management-cluster"

  setting {
    name  = "containerInsights"
    value = "enabled"
  }

  tags = {
    Name = "employee-management-cluster"
  }
}

# ECS Task Definition for Backend
resource "aws_ecs_task_definition" "backend" {
  family                   = "employee-management-backend"
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = "512"
  memory                   = "1024"
  execution_role_arn       = aws_iam_role.ecs_task_execution_role.arn
  task_role_arn            = aws_iam_role.ecs_task_role.arn

  container_definitions = jsonencode([{
    name      = "backend"
    image     = "${aws_ecr_repository.backend.repository_url}:latest"
    essential = true

    portMappings = [{
      containerPort = 8080
      hostPort      = 8080
      protocol      = "tcp"
    }]

    environment = [
      {
        name  = "SPRING_DATASOURCE_URL"
        value = "jdbc:mysql://${aws_db_instance.mysql.endpoint}/employee_management"
      },
      {
        name  = "SPRING_DATASOURCE_USERNAME"
        value = aws_db_instance.mysql.username
      },
      {
        name  = "SPRING_REDIS_HOST"
        value = aws_elasticache_cluster.redis.cache_nodes[0].address
      }
    ]

    secrets = [
      {
        name      = "SPRING_DATASOURCE_PASSWORD"
        valueFrom = aws_secretsmanager_secret_version.db_password.arn
      },
      {
        name      = "JWT_SECRET"
        valueFrom = aws_secretsmanager_secret_version.jwt_secret.arn
      }
    ]

    logConfiguration = {
      logDriver = "awslogs"
      options = {
        "awslogs-group"         = aws_cloudwatch_log_group.backend.name
        "awslogs-region"        = "us-east-1"
        "awslogs-stream-prefix" = "ecs"
      }
    }
  }])

  tags = {
    Name = "employee-management-backend"
  }
}

# Application Load Balancer
resource "aws_lb" "main" {
  name               = "employee-management-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = aws_subnet.public[*].id

  enable_deletion_protection = true
  enable_http2              = true
  enable_cross_zone_load_balancing = true

  tags = {
    Name = "employee-management-alb"
  }
}

# Target Group
resource "aws_lb_target_group" "backend" {
  name        = "employee-management-tg"
  port        = 8080
  protocol    = "HTTP"
  vpc_id      = aws_vpc.main.id
  target_type = "ip"

  health_check {
    healthy_threshold   = 2
    unhealthy_threshold = 2
    timeout             = 3
    interval            = 30
    path                = "/actuator/health"
    matcher             = "200"
  }

  tags = {
    Name = "employee-management-tg"
  }
}

# CloudFront Distribution
resource "aws_cloudfront_distribution" "main" {
  enabled = true

  origin {
    domain_name = aws_lb.main.dns_name
    origin_id   = "ALB"

    custom_origin_config {
      http_port             = 80
      https_port            = 443
      origin_protocol_policy = "http-only"
      origin_ssl_protocols   = ["TLSv1.2"]
    }
  }

  default_cache_behavior {
    cache_policy_id  = data.aws_cloudfront_cache_policy.optimized.id
    allowed_methods  = ["GET", "HEAD", "OPTIONS", "PUT", "POST", "PATCH", "DELETE"]
    cached_methods   = ["GET", "HEAD"]
    compress         = true
    target_origin_id = "ALB"
    viewer_protocol_policy = "redirect-to-https"
  }

  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }

  viewer_certificate {
    acm_certificate_arn      = aws_acm_certificate.main.arn
    ssl_support_method       = "sni-only"
    minimum_protocol_version = "TLSv1.2_2021"
  }

  tags = {
    Name = "employee-management-cdn"
  }
}

# Auto Scaling Group
resource "aws_autoscaling_group" "backend" {
  name            = "employee-management-asg"
  max_size        = 10
  min_size        = 2
  desired_capacity = 3

  vpc_zone_identifier = aws_subnet.public[*].id

  tag {
    key                 = "Name"
    value               = "employee-management-backend"
    propagate_at_launch = true
  }
}

# Outputs
output "cloudfront_distribution_domain_name" {
  value = aws_cloudfront_distribution.main.domain_name
}

output "rds_endpoint" {
  value = aws_db_instance.mysql.endpoint
}

output "redis_endpoint" {
  value = aws_elasticache_cluster.redis.cache_nodes[0].address
}
```

### Deployment Commands

```bash
# Initialize Terraform
terraform init

# Plan deployment
terraform plan -out=tfplan

# Apply configuration
terraform apply tfplan

# Get outputs
terraform output

# Destroy infrastructure (when done)
terraform destroy
```

---

## Azure Deployment

### Architecture with Azure App Service

```
┌─────────────────────────────────────────────┐
│         Azure Front Door (CDN + WAF)        │
└──────────────────┬──────────────────────────┘
                   │
    ┌──────────────┼──────────────┐
    │              │              │
┌───▼───┐     ┌───▼───┐     ┌───▼────┐
│App Svc│     │App Svc│     │App Svc │
│(East) │     │(West) │     │(Europe)│
└───┬───┘     └───┬───┘     └───┬────┘
    │             │             │
    │     ┌───────┴─────────┐   │
    │     │                 │   │
    ▼     ▼                 ▼   ▼
    ┌──────────────────────────┐
    │  Azure Database MySQL    │
    │  (Multi-region replicas) │
    └──────┬───────────────────┘
           │
    ┌──────▼──────────────────┐
    │  Azure Cache for Redis  │
    └────────────────────────┘
           │
    ┌──────▼──────────────────┐
    │  Blob Storage (Backup)  │
    └────────────────────────┘
```

### Azure Bicep Template

```bicep
// main.bicep

param location string = resourceGroup().location
param environmentName string
param instanceCount int = 2

// Resource Group
resource appServicePlan 'Microsoft.Web/serverfarms@2022-03-01' = {
  name: 'asp-employee-mgmt-${environmentName}'
  location: location
  sku: {
    name: 'P1V2'
    capacity: 1
  }
  kind: 'linux'
  properties: {
    reserved: true
  }
}

// Frontend App Service
resource frontendAppService 'Microsoft.Web/sites@2022-03-01' = {
  name: 'app-frontend-${environmentName}'
  location: location
  kind: 'app,linux,container'
  properties: {
    serverFarmId: appServicePlan.id
    siteConfig: {
      linuxFxVersion: 'NODE|18-lts'
      appSettings: [
        {
          name: 'REACT_APP_API_URL'
          value: 'https://app-backend-${environmentName}.azurewebsites.net'
        }
        {
          name: 'PORT'
          value: '3000'
        }
      ]
    }
  }
}

// Backend App Service
resource backendAppService 'Microsoft.Web/sites@2022-03-01' = {
  name: 'app-backend-${environmentName}'
  location: location
  kind: 'app,linux,container'
  properties: {
    serverFarmId: appServicePlan.id
    siteConfig: {
      linuxFxVersion: 'JAVA|17-java17'
      appSettings: [
        {
          name: 'SPRING_DATASOURCE_URL'
          value: 'jdbc:mysql://${mySqlServer.properties.fullyQualifiedDomainName}:3306/employee_management'
        }
        {
          name: 'SPRING_REDIS_HOST'
          value: redis.properties.hostName
        }
        {
          name: 'PORT'
          value: '8080'
        }
      ]
    }
  }
}

// MySQL Server
resource mySqlServer 'Microsoft.DBforMySQL/servers@2017-12-01' = {
  name: 'mysql-employee-mgmt-${environmentName}'
  location: location
  sku: {
    name: 'B_Gen5_1'
    tier: 'Basic'
    capacity: 1
    family: 'Gen5'
  }
  properties: {
    createMode: 'Default'
    version: '8.0'
    sslEnforcement: 'ENABLED'
    administratorLogin: 'sqladmin'
    administratorLoginPassword: uniqueString(resourceGroup().id)
  }
}

// Azure Cache for Redis
resource redis 'Microsoft.Cache/redis@2022-06-01' = {
  name: 'redis-employee-mgmt-${environmentName}'
  location: location
  properties: {
    sku: {
      name: 'Basic'
      family: 'C'
      capacity: 0
    }
    enableNonSslPort: false
    minimumTlsVersion: '1.2'
  }
}

// Front Door
resource frontDoor 'Microsoft.Network/frontDoors@2021-06-01' = {
  name: 'fd-employee-mgmt-${environmentName}'
  location: 'global'
  properties: {
    enabledState: 'Enabled'
    frontendEndpoints: [
      {
        name: 'frontend-endpoint'
        properties: {
          hostName: 'employee-mgmt-${environmentName}.azurefd.net'
          sessionAffinityEnabledState: 'Disabled'
        }
      }
    ]
    backendPools: [
      {
        name: 'backend-pool'
        properties: {
          backends: [
            {
              address: backendAppService.properties.defaultHostName
              httpPort: 80
              httpsPort: 443
              priority: 1
              weight: 50
              backendHostHeader: backendAppService.properties.defaultHostName
            }
          ]
        }
      }
    ]
    routingRules: [
      {
        name: 'routing-rule'
        properties: {
          frontendEndpoints: [
            {
              id: resourceId('Microsoft.Network/frontDoors/frontendEndpoints', frontDoor.name, 'frontend-endpoint')
            }
          ]
          acceptedProtocols: ['Http', 'Https']
          patternsToMatch: ['/*']
          routeConfiguration: {
            oDataType: '#Microsoft.Azure.FrontDoor.Models.FrontdoorForwardingConfiguration'
            forwardingProtocol: 'HttpsOnly'
            backendPool: {
              id: resourceId('Microsoft.Network/frontDoors/backendPools', frontDoor.name, 'backend-pool')
            }
          }
          enabledState: 'Enabled'
        }
      }
    ]
  }
}

output frontendUrl string = 'https://${frontendAppService.properties.defaultHostName}'
output backendUrl string = 'https://${backendAppService.properties.defaultHostName}'
output mySqlEndpoint string = mySqlServer.properties.fullyQualifiedDomainName
output redisEndpoint string = redis.properties.hostName
```

### Deployment Commands

```bash
# Azure CLI deployment
az group create --name rg-employee-mgmt --location eastus

az deployment group create \
  --resource-group rg-employee-mgmt \
  --template-file main.bicep \
  --parameters environmentName=prod instanceCount=3

# Get connection strings
az mysql server show --resource-group rg-employee-mgmt --name mysql-employee-mgmt-prod

# Scale resources
az appservice plan update --resource-group rg-employee-mgmt \
  --name asp-employee-mgmt-prod --sku P2V2
```

---

## Kubernetes Deployment

### Helm Chart Structure

```
employee-management-helm/
├── Chart.yaml
├── values.yaml
├── values-prod.yaml
├── values-dev.yaml
├── templates/
│   ├── namespace.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   ├── pvc.yaml
│   ├── mysql/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── statefulset.yaml
│   ├── redis/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── backend/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── ingress.yaml
│   │   └── hpa.yaml
│   └── frontend/
│       ├── deployment.yaml
│       ├── service.yaml
│       └── ingress.yaml
```

### Helm values-prod.yaml

```yaml
# Production values
namespace: employee-management-prod

# Backend Configuration
backend:
  image:
    repository: myregistry.azurecr.io/employee-management-backend
    tag: "1.0.0"
    pullPolicy: Always
  
  replicas: 3
  
  resources:
    requests:
      memory: "512Mi"
      cpu: "250m"
    limits:
      memory: "1Gi"
      cpu: "500m"
  
  env:
    SPRING_PROFILES_ACTIVE: "prod"
    SPRING_JPA_HIBERNATE_DDL_AUTO: "validate"
  
  livenessProbe:
    httpGet:
      path: /actuator/health
      port: 8080
    initialDelaySeconds: 30
    periodSeconds: 10
  
  readinessProbe:
    httpGet:
      path: /actuator/health/readiness
      port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5
  
  autoscaling:
    enabled: true
    minReplicas: 3
    maxReplicas: 10
    targetCPUUtilizationPercentage: 70
    targetMemoryUtilizationPercentage: 80
  
  service:
    type: ClusterIP
    port: 8080

# Frontend Configuration
frontend:
  image:
    repository: myregistry.azurecr.io/employee-management-frontend
    tag: "1.0.0"
    pullPolicy: Always
  
  replicas: 3
  
  resources:
    requests:
      memory: "256Mi"
      cpu: "100m"
    limits:
      memory: "512Mi"
      cpu: "200m"
  
  service:
    type: ClusterIP
    port: 3000

# MySQL Configuration
mysql:
  enabled: true
  auth:
    rootPassword: "changeMe123!"
    database: "employee_management"
    username: "empuser"
    password: "emppass123!"
  
  primary:
    persistence:
      enabled: true
      size: 100Gi
      storageClassName: "premium-rwo"
  
  replica:
    replicaCount: 2
    persistence:
      enabled: true
      size: 100Gi
      storageClassName: "premium-rwo"

# Redis Configuration
redis:
  enabled: true
  architecture: replication
  replica:
    replicaCount: 2
  
  auth:
    enabled: true
    password: "redispass123!"
  
  persistence:
    enabled: true
    size: 10Gi
    storageClassName: "premium-rwo"

# Ingress Configuration
ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/rate-limit: "100"
  
  hosts:
    - host: api.example.com
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: backend
              port:
                number: 8080
    - host: www.example.com
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: frontend
              port:
                number: 3000
  
  tls:
    - secretName: employee-mgmt-tls
      hosts:
        - api.example.com
        - www.example.com

# Monitoring
monitoring:
  enabled: true
  prometheus:
    enabled: true
  grafana:
    enabled: true
```

### Deployment Commands

```bash
# Add Helm repository
helm repo add myrepo https://helm.example.com
helm repo update

# Install release
helm install employee-management myrepo/employee-management \
  --namespace employee-management-prod \
  --create-namespace \
  -f values-prod.yaml

# Upgrade release
helm upgrade employee-management myrepo/employee-management \
  --namespace employee-management-prod \
  -f values-prod.yaml

# Rollback
helm rollback employee-management 1 \
  --namespace employee-management-prod

# View status
helm status employee-management -n employee-management-prod

# Check pod status
kubectl get pods -n employee-management-prod
kubectl logs -f pod/backend-xyz -n employee-management-prod

# Port forward for testing
kubectl port-forward svc/backend 8080:8080 -n employee-management-prod
```

---

## CI/CD Pipelines

### GitHub Actions Workflow

```yaml
# .github/workflows/deploy-prod.yml

name: Deploy to Production

on:
  push:
    branches: [ main ]
  release:
    types: [ published ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      mysql:
        image: mysql:8.3
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: test_db
        options: >-
          --health-cmd="mysqladmin ping -u root -proot"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=3
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Java
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: maven
      
      - name: Run backend tests
        run: cd backend && mvn clean test
      
      - name: Set up Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      
      - name: Run frontend tests
        run: cd frontend && npm ci && npm run test

  build:
    needs: test
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Log in to Container Registry
        uses: docker/login-action@v2
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and push backend image
        uses: docker/build-push-action@v4
        with:
          context: ./backend
          push: true
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}/backend:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max
      
      - name: Build and push frontend image
        uses: docker/build-push-action@v4
        with:
          context: ./frontend
          push: true
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}/frontend:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.event_name == 'release' && github.event.action == 'published'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Kubernetes
        env:
          KUBECONFIG: ${{ secrets.KUBE_CONFIG }}
        run: |
          mkdir -p $HOME/.kube
          echo "$KUBECONFIG" | base64 -d > $HOME/.kube/config
          kubectl set image deployment/backend backend=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}/backend:latest -n employee-management-prod
          kubectl set image deployment/frontend frontend=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}/frontend:latest -n employee-management-prod
          kubectl rollout status deployment/backend -n employee-management-prod
          kubectl rollout status deployment/frontend -n employee-management-prod
      
      - name: Run smoke tests
        run: |
          sleep 30
          curl -f http://api.example.com/actuator/health || exit 1
```

---

## Monitoring & Logging

### Prometheus + Grafana Setup

```yaml
# docker-compose-monitoring.yml

version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana-data:/var/lib/grafana
      - ./grafana/dashboards:/etc/grafana/provisioning/dashboards
      - ./grafana/datasources:/etc/grafana/provisioning/datasources

  loki:
    image: grafana/loki:latest
    ports:
      - "3100:3100"
    volumes:
      - ./loki-config.yaml:/etc/loki/local-config.yaml
      - loki-data:/loki

  promtail:
    image: grafana/promtail:latest
    volumes:
      - /var/log:/var/log
      - ./promtail-config.yaml:/etc/promtail/config.yml
    command: -config.file=/etc/promtail/config.yml

volumes:
  prometheus-data:
  grafana-data:
  loki-data:
```

### Prometheus Configuration

```yaml
# prometheus.yml

global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'backend'
    static_configs:
      - targets: ['localhost:8080']
    metrics_path: '/actuator/prometheus'

  - job_name: 'mysql'
    static_configs:
      - targets: ['localhost:33306']

  - job_name: 'redis'
    static_configs:
      - targets: ['localhost:6379']
```

---

## Performance Tuning

### Database Optimization

```sql
-- Add indexes for better query performance
CREATE INDEX idx_employee_email ON employee(email);
CREATE INDEX idx_employee_department ON employee(department_id);
CREATE INDEX idx_employee_created_at ON employee(created_at);

-- Enable query cache
SET GLOBAL query_cache_type = ON;
SET GLOBAL query_cache_size = 268435456; -- 256 MB

-- Optimize table structure
ALTER TABLE employee ROW_FORMAT=COMPRESSED;
ALTER TABLE department ROW_FORMAT=COMPRESSED;
```

---

**Last Updated:** November 17, 2025
**Author:** Kodati Sai Teja
**GitHub:** https://github.com/TEJA6777
