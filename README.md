# MEAN Stack Application - DevOps CI/CD Project

[![CI/CD Pipeline](https://github.com/abhinraj-hub/devopsproject/actions/workflows/deploy.yml/badge.svg)](https://github.com/abhinraj-hub/devopsproject/actions)
[![Docker](https://img.shields.io/badge/Docker-Enabled-blue)](https://hub.docker.com/u/abhinr)
[![AWS](https://img.shields.io/badge/AWS-EC2-orange)](http://3.111.43.151)

## 📋 Table of Contents

- [Overview](#overview)
- [Live Demo](#live-demo)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
  - [Local Development](#local-development)
  - [Production Deployment](#production-deployment)
- [CI/CD Pipeline](#cicd-pipeline)
- [Docker Configuration](#docker-configuration)
- [Environment Variables](#environment-variables)
- [API Documentation](#api-documentation)
- [Deployment Screenshots](#deployment-screenshots)
- [Troubleshooting](#troubleshooting)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## 🎯 Overview

This project is a full-stack **MEAN (MongoDB, Express.js, Angular, Node.js)** application demonstrating modern DevOps practices including:

- **Containerization** using Docker
- **Container Orchestration** with Docker Compose
- **Automated CI/CD Pipeline** using GitHub Actions
- **Cloud Deployment** on AWS EC2
- **Reverse Proxy** configuration with Nginx
- **Container Registry** management on Docker Hub

**Assignment for:** DevOps Engineer Internship at Discover Dollar Inc  
**Submission Date:** December 2024  
**Candidate:** [Your Name]

---

## 🌐 Live Demo

| Service | URL | Description |
|---------|-----|-------------|
| **Main Application** | http://3.111.43.151 | Nginx reverse proxy (Port 80) |
| **Frontend (Direct)** | http://3.111.43.151:4200 | Angular application |
| **Backend API** | http://3.111.43.151:5000 | RESTful API endpoints |

---

## 🛠 Technology Stack

### Frontend
- **Framework:** Angular 15
- **Language:** TypeScript
- **Styling:** Bootstrap 4.6
- **Build Tool:** Angular CLI
- **Web Server:** Nginx (Alpine)

### Backend
- **Runtime:** Node.js 18 (Alpine)
- **Framework:** Express.js
- **Language:** JavaScript
- **API:** RESTful architecture

### Database
- **Database:** MongoDB (Latest)
- **Storage:** Persistent volume using Docker volumes

### DevOps Tools
- **Containerization:** Docker
- **Orchestration:** Docker Compose
- **CI/CD:** GitHub Actions
- **Cloud Platform:** AWS EC2 (Ubuntu 20.04)
- **Reverse Proxy:** Nginx
- **Container Registry:** Docker Hub
- **Version Control:** Git & GitHub

---

## 🏗 Architecture

### System Architecture Diagram
```
                          Internet
                             |
                    [Domain/IP: 3.111.43.151]
                             |
                    +--------v--------+
                    |   Nginx :80     |  ← Reverse Proxy
                    |   (Host OS)     |
                    +--------+--------+
                             |
        +--------------------+--------------------+
        |                    |                    |
  +-----v------+      +------v-----+      +------v------+
  |  Frontend  |      |  Backend   |      |  MongoDB    |
  |  Angular   |----->|  Node.js   |----->|  Database   |
  |  Port:4200 |      |  Port:5000 |      |  Port:27017 |
  +------------+      +------------+      +-------------+
        |                    |                    |
        +--------------------+--------------------+
                             |
                    Docker Bridge Network
                             |
                    [AWS EC2 Ubuntu Instance]
```

### CI/CD Pipeline Flow
```
Developer Push
      |
      v
+---------------------+
|   GitHub Repo       |
+---------------------+
      |
      v (Webhook Trigger)
+---------------------+
| GitHub Actions      |
| - Checkout Code     |
| - Build Images      |
| - Push to DockerHub |
| - SSH to AWS        |
+---------------------+
      |
      v
+---------------------+
|  Docker Hub         |
|  Image Registry     |
+---------------------+
      |
      v
+---------------------+
|  AWS EC2 Instance   |
|  - Pull Images      |
|  - Restart Containers|
|  - Serve Application|
+---------------------+
```

---

## ✨ Features

### Application Features
- ✅ **CRUD Operations** - Create, Read, Update, Delete tutorials
- ✅ **Search Functionality** - Search tutorials by title
- ✅ **Responsive UI** - Mobile-friendly interface
- ✅ **RESTful API** - Well-structured backend endpoints
- ✅ **Real-time Updates** - Immediate data synchronization

### DevOps Features
- ✅ **Fully Containerized** - All services run in Docker containers
- ✅ **Automated Deployment** - Push to GitHub triggers automatic deployment
- ✅ **Zero Downtime Updates** - Rolling updates with Docker Compose
- ✅ **Persistent Data** - MongoDB data survives container restarts
- ✅ **Scalable Architecture** - Easy to scale individual services
- ✅ **Production-Ready** - Nginx reverse proxy with proper configuration

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

### For Local Development:
- **Docker:** Version 20.10 or higher
- **Docker Compose:** Version 2.0 or higher
- **Git:** Version 2.30 or higher
- **Node.js:** Version 18.x (if running without Docker)

### For Production Deployment:
- **AWS Account** with EC2 access
- **Docker Hub Account**
- **GitHub Account**
- **SSH Key Pair** for AWS EC2

### Installation Links:
- Docker: https://docs.docker.com/get-docker/
- Docker Compose: https://docs.docker.com/compose/install/
- Git: https://git-scm.com/downloads
- Node.js: https://nodejs.org/

---

## 🚀 Installation & Setup

### Local Development

#### 1. Clone the Repository
```bash
git clone https://github.com/abhinraj-hub/devopsproject.git
cd devopsproject
```

#### 2. Start the Application
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Check container status
docker ps
```

#### 3. Access the Application

- **Frontend:** http://localhost:4200
- **Backend API:** http://localhost:5000
- **MongoDB:** localhost:27017

#### 4. Stop the Application
```bash
docker-compose down

# To remove volumes as well (clears database)
docker-compose down -v
```

---

### Production Deployment

#### Step 1: Launch AWS EC2 Instance
```bash
# Instance specifications:
- AMI: Ubuntu 20.04 LTS
- Instance Type: t2.micro (or higher)
- Storage: 20 GB minimum
- Security Groups: Ports 80, 4200, 5000, 8080, 22
```

#### Step 2: Connect to EC2 Instance
```bash
ssh -i your-key.pem ubuntu@3.111.43.151
```

#### Step 3: Install Docker & Docker Compose
```bash
# Update system
sudo apt-get update && sudo apt-get upgrade -y

# Install Docker
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verify installations
docker --version
docker-compose --version
```

#### Step 4: Install Nginx
```bash
# Install Nginx
sudo apt-get install nginx -y

# Remove default configuration
sudo rm /etc/nginx/sites-enabled/default

# Create new configuration
sudo nano /etc/nginx/sites-available/meanapp
```

**Nginx Configuration:**
```nginx
server {
    listen 80;
    server_name _;

    # Frontend - Angular Application
    location / {
        proxy_pass http://localhost:4200;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Backend API
    location /api/ {
        proxy_pass http://localhost:5000/api/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Enable and restart Nginx:**
```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/meanapp /etc/nginx/sites-enabled/

# Test configuration
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```

#### Step 5: Clone and Deploy
```bash
# Clone repository
git clone https://github.com/abhinraj-hub/devopsproject.git
cd devopsproject

# Start application
docker-compose up -d

# Verify containers are running
docker ps
```

#### Step 6: Configure AWS Security Group

| Type | Port | Source | Description |
|------|------|--------|-------------|
| HTTP | 80 | 0.0.0.0/0 | Nginx reverse proxy |
| Custom TCP | 4200 | 0.0.0.0/0 | Frontend (optional) |
| Custom TCP | 5000 | 0.0.0.0/0 | Backend API (optional) |
| SSH | 22 | Your IP | SSH access |

---

## 🔄 CI/CD Pipeline

### Pipeline Overview

The CI/CD pipeline is implemented using **GitHub Actions** and automatically triggers on every push to the `master` branch.

### Pipeline Stages

#### 1. **Build and Push Images**
```yaml
- Checkout code from GitHub
- Set up Docker Buildx
- Login to Docker Hub
- Build backend Docker image
- Push backend image to Docker Hub
- Build frontend Docker image
- Push frontend image to Docker Hub
```

#### 2. **Deploy to AWS**
```yaml
- SSH into AWS EC2 instance
- Pull latest code from GitHub
- Pull latest Docker images
- Restart containers with new images
- Clean up old images
```

### GitHub Secrets Configuration

Navigate to: **Repository → Settings → Secrets and variables → Actions**

Add these secrets:

| Secret Name | Description | Example Value |
|-------------|-------------|---------------|
| `DOCKERHUB_USERNAME` | Docker Hub username | `abhinr` |
| `DOCKERHUB_TOKEN` | Docker Hub access token | `dckr_pat_xxxxx` |
| `AWS_HOST` | AWS EC2 public IP | `3.111.43.151` |
| `AWS_USERNAME` | SSH username | `root` or `ubuntu` |
| `AWS_SSH_KEY` | Private SSH key | Full content of `.pem` file |

### Triggering the Pipeline

**Automatic Trigger:**
```bash
git add .
git commit -m "Your commit message"
git push origin master
```

**Manual Trigger:**
- Go to GitHub → Actions tab
- Select "CI/CD Pipeline"
- Click "Run workflow"

### Pipeline Execution Time

| Stage | Average Time |
|-------|--------------|
| Checkout & Setup | 10 seconds |
| Build Backend | 30 seconds |
| Build Frontend | 1-2 minutes |
| Push to Docker Hub | 30 seconds |
| Deploy to AWS | 30 seconds |
| **Total** | **~3-4 minutes** |

---

## 🐳 Docker Configuration

### Backend Dockerfile

**Location:** `backend/Dockerfile`
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 5000

CMD ["npm", "start"]
```

### Frontend Dockerfile

**Location:** `frontend/Dockerfile`
```dockerfile
# Build stage
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install --legacy-peer-deps

COPY . .
RUN npm run build -- --configuration production

# Production stage
FROM nginx:alpine

COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Docker Compose Configuration

**Location:** `docker-compose.yml`
```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    restart: always
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    environment:
      MONGO_INITDB_DATABASE: meanapp
    networks:
      - app-network

  backend:
    image: abhinr/meanapp-backend:latest
    container_name: backend
    restart: always
    ports:
      - "5000:5000"
    depends_on:
      - mongodb
    environment:
      MONGO_URI: mongodb://mongodb:27017/meanapp
      PORT: 5000
      NODE_ENV: production
    networks:
      - app-network

  frontend:
    image: abhinr/meanapp-frontend:latest
    container_name: frontend
    restart: always
    ports:
      - "4200:80"
    depends_on:
      - backend
    networks:
      - app-network

volumes:
  mongo-data:

networks:
  app-network:
    driver: bridge
```

### Docker Hub Images

- **Backend:** [abhinr/meanapp-backend:latest](https://hub.docker.com/r/abhinr/meanapp-backend)
- **Frontend:** [abhinr/meanapp-frontend:latest](https://hub.docker.com/r/abhinr/meanapp-frontend)

---

## 🔐 Environment Variables

### Backend Environment Variables

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `MONGO_URI` | MongoDB connection string | `mongodb://mongodb:27017/meanapp` |
| `PORT` | Backend server port | `5000` |
| `NODE_ENV` | Node environment | `production` |

### Frontend Environment Variables

Located in `src/environments/environment.prod.ts`:
```typescript
export const environment = {
  production: true,
  apiUrl: '/api'
};
```

---

## 📡 API Documentation

### Base URL
```
http://3.111.43.151:5000/api
```

### Endpoints

#### Tutorials

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tutorials` | Get all tutorials |
| GET | `/api/tutorials/:id` | Get tutorial by ID |
| POST | `/api/tutorials` | Create new tutorial |
| PUT | `/api/tutorials/:id` | Update tutorial |
| DELETE | `/api/tutorials/:id` | Delete tutorial |
| DELETE | `/api/tutorials` | Delete all tutorials |
| GET | `/api/tutorials?title=keyword` | Search tutorials |

#### Example Requests

**Create Tutorial:**
```bash
curl -X POST http://3.111.43.151:5000/api/tutorials \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Docker Tutorial",
    "description": "Learn Docker containerization"
  }'
```

**Get All Tutorials:**
```bash
curl http://3.111.43.151:5000/api/tutorials
```

---

### 1. Application User Interface

**Description:** MEAN Stack application running on AWS EC2 with full CRUD functionality.

---

### 2. GitHub Actions CI/CD Pipeline

**Description:** Automated CI/CD pipeline showing successful execution with all stages completed.

**Pipeline Stages:**
- ✅ Checkout code (8s)
- ✅ Login to Docker Hub (1s)
- ✅ Build and push backend (30s)
- ✅ Build and push frontend (1m 14s)
- ✅ Deploy to AWS (25s)

---

### 3. Docker Hub Registry

**Description:** Docker images published to Docker Hub registry for backend and frontend services.

**Images:**
- `abhinr/meanapp-backend:latest`
- `abhinr/meanapp-frontend:latest`

---

### 4. Running Docker Containers

**Container Status:**
```
CONTAINER ID   IMAGE                          STATUS        PORTS
abc123         abhinr/meanapp-frontend        Up 2 hours    0.0.0.0:4200->80/tcp
def456         abhinr/meanapp-backend         Up 2 hours    0.0.0.0:5000->5000/tcp
ghi789         mongo:latest                   Up 2 hours    0.0.0.0:27017->27017/tcp
```

---

### 5. AWS EC2 Instance

**Instance Details:**
- **Instance Type:** t2.micro
- **OS:** Ubuntu 20.04 LTS
- **Public IP:** 3.111.43.151
- **Status:** Running
- **Security Groups:** HTTP (80), Custom TCP (4200, 5000)

---

### 6. Nginx Reverse Proxy Configuration

**Configuration Highlights:**
- Listens on port 80
- Proxies requests to frontend (port 4200)
- Routes `/api/*` to backend (port 5000)
- Proper headers for WebSocket support

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### Issue 1: Containers Not Starting

**Symptoms:**
```bash
docker ps  # Shows no containers
```

**Solution:**
```bash
# Check logs
docker-compose logs

# Restart services
docker-compose down
docker-compose up -d

# Check for port conflicts
sudo lsof -i :4200
sudo lsof -i :5000
sudo lsof -i :27017
```

---

#### Issue 2: MongoDB Connection Failed

**Symptoms:**
```
MongoNetworkError: failed to connect to server
```

**Solution:**
```bash
# Check MongoDB is running
docker logs mongodb

# Verify network connectivity
docker network inspect devopsproject_app-network

# Restart MongoDB
docker-compose restart mongodb
```

---

#### Issue 3: Frontend Shows 404

**Symptoms:**
- Application loads but shows 404 on refresh

**Solution:**
```bash
# Check Nginx configuration
sudo nginx -t

# Ensure try_files is configured
# See frontend/nginx.conf

# Restart Nginx
sudo systemctl restart nginx
```

---

#### Issue 4: CI/CD Pipeline Fails

**Symptoms:**
- GitHub Actions shows red X

**Common Causes & Solutions:**

**A. Docker Hub Login Failed**
```yaml
# Verify secrets are set correctly:
# DOCKERHUB_USERNAME
# DOCKERHUB_TOKEN
```

**B. SSH Connection Failed**
```yaml
# Verify secrets:
# AWS_HOST = correct IP
# AWS_USERNAME = root or ubuntu
# AWS_SSH_KEY = full private key with BEGIN/END lines
```

**C. Deployment Script Failed**
```bash
# SSH to server and test commands manually:
cd /root/devopsproject
git pull origin master
docker-compose pull
docker-compose up -d
```

---

#### Issue 5: Application Not Accessible

**Symptoms:**
```
This site can't be reached
```

**Solution:**
```bash
# 1. Check containers are running
docker ps

# 2. Check Nginx is running
sudo systemctl status nginx

# 3. Verify AWS Security Group
# Ensure port 80 is open to 0.0.0.0/0

# 4. Test locally first
curl http://localhost:80

# 5. Check firewall
sudo ufw status
```

---

### Useful Commands

#### Docker Commands
```bash
# View all containers (including stopped)
docker ps -a

# View logs for specific container
docker logs backend
docker logs frontend
docker logs mongodb

# Follow logs in real-time
docker-compose logs -f

# Restart specific service
docker-compose restart backend

# Rebuild containers
docker-compose up -d --build

# Remove all containers and volumes
docker-compose down -v

# Clean up unused images
docker system prune -a -f

# Check Docker disk usage
docker system df
```

#### Git Commands
```bash
# Check current branch
git branch

# Pull latest changes
git pull origin master

# View commit history
git log --oneline -10

# Check remote URL
git remote -v
```

#### Nginx Commands
```bash
# Test configuration
sudo nginx -t

# Reload configuration
sudo nginx -s reload

# Restart Nginx
sudo systemctl restart nginx

# View error logs
sudo tail -f /var/log/nginx/error.log

# View access logs
sudo tail -f /var/log/nginx/access.log
```

#### System Monitoring
```bash
# Check disk space
df -h

# Check memory usage
free -h

# Check running processes
top

# Check specific port
sudo netstat -tulpn | grep :80

# Check system logs
journalctl -xe
```

---

## 📁 Project Structure
```
devopsproject/
│
├── .github/
│   └── workflows/
│       └── deploy.yml              # GitHub Actions CI/CD pipeline
│
├── backend/
│   ├── app/
│   │   ├── config/
│   │   │   └── db.config.js       # Database configuration
│   │   ├── controllers/
│   │   │   └── tutorial.controller.js
│   │   ├── models/
│   │   │   └── tutorial.model.js
│   │   └── routes/
│   │       └── tutorial.routes.js
│   ├── Dockerfile                  # Backend container configuration
│   ├── package.json               # Backend dependencies
│   └── server.js                  # Backend entry point
│
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── components/
│   │   │   ├── models/
│   │   │   └── services/
│   │   ├── environments/
│   │   │   ├── environment.ts
│   │   │   └── environment.prod.ts
│   │   └── index.html
│   ├── Dockerfile                  # Frontend container configuration
│   ├── nginx.conf                  # Nginx configuration for frontend
│   ├── package.json               # Frontend dependencies
│   └── angular.json               # Angular configuration
│
├── screenshots/                    # Documentation screenshots
│   ├── app-ui.png
│   ├── github-actions.png
│   ├── docker-hub.png
│   ├── docker-ps.png
│   ├── aws-instance.png
│   └── nginx-config.png
│
├── docker-compose.yml             # Multi-container orchestration
└── README.md                      # Project documentation (this file)
```

---
### 1. Fork the Repository
```bash
# Click "Fork" button on GitHub
```

### 2. Clone Your Fork
```bash
git clone https://github.com/YOUR_USERNAME/devopsproject.git
cd devopsproject
```

### 3. Create a Feature Branch
```bash
git checkout -b feature/your-feature-name
```

### 4. Make Changes
```bash
# Make your changes
git add .
git commit -m "Add: your feature description"
```

### 5. Push to Your Fork
```bash
git push origin feature/your-feature-name
```

### 6. Create Pull Request

- Go to original repository
- Click "New Pull Request"
- Select your branch
- Describe your changes

### Code Style Guidelines

- **JavaScript/TypeScript:** Follow Airbnb style guide
- **Docker:** Use multi-stage builds where possible
- **Documentation:** Update README for any new features
- **Commits:** Use conventional commit messages

---
