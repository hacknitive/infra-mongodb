<p align="center">
  <img src="./assets/avatar.jpeg" alt="MongoDB Docker Infrastructure" width="200">
</p>

<h1 align="center">🍃 Production-Ready MongoDB Docker Infrastructure</h1>

<p align="center">
  <strong>A complete, optimized, and secure MongoDB deployment solution using Docker</strong>
</p>

<p align="center">
  <a href="https://github.com/hacknitive/infra-mongodb">
    <img src="https://img.shields.io/badge/GitHub-infra--mongodb-blue?logo=github" alt="GitHub Repository">
  </a>
  <a href="https://www.mongodb.com/">
    <img src="https://img.shields.io/badge/MongoDB-7.0-green?logo=mongodb" alt="MongoDB 7.0">
  </a>
  <a href="https://www.docker.com/">
    <img src="https://img.shields.io/badge/Docker-24.0+-blue?logo=docker" alt="Docker">
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License">
  </a>
</p>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Configuration Guide](#-configuration-guide)
- [Deployment Options](#-deployment-options)
- [Connecting to MongoDB](#-connecting-to-mongodb)
- [Management Commands](#-management-commands)
- [CI/CD Integration](#-cicd-integration)
- [Performance Tuning](#-performance-tuning)
- [Security Best Practices](#-security-best-practices)
- [Troubleshooting](#-troubleshooting)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Overview

This repository provides a **production-ready MongoDB 7.0 infrastructure** with Docker, featuring enterprise-grade security, performance optimization, and comprehensive configuration management. Perfect for development, staging, and production environments.

**Repository:** [https://github.com/hacknitive/infra-mongodb](https://github.com/hacknitive/infra-mongodb)

### Why Use This Project?

✅ **Ready to Deploy** - Copy templates, configure, and launch in minutes  
✅ **Production Hardened** - Security best practices built-in  
✅ **Highly Configurable** - 40+ environment variables for fine-tuning  
✅ **CI/CD Ready** - GitLab CI/CD pipeline templates included  
✅ **Well Documented** - Comprehensive inline documentation in all files  
✅ **Resource Optimized** - CPU, memory, and I/O constraints configured  

---

## ✨ Features

### 🔐 Security
- **Authentication enabled by default** with customizable root credentials
- **Minimal container capabilities** (drops all, adds only necessary ones)
- **Unprivileged execution** as mongodb user (UID 999)
- **Security hardening** following Docker and MongoDB best practices

### 🚀 Performance
- **WiredTiger storage engine** with configurable cache size
- **Data compression** (snappy/zstd) for storage and network traffic
- **Resource constraints** for CPU, memory, and I/O management
- **Optimized journaling** with configurable commit intervals
- **Index optimization** with prefix compression

### 📊 Operations
- **Health checks** for container orchestration
- **Log rotation** with configurable size and retention
- **Persistent volumes** for data and logs
- **Automated backups** support via volume snapshots
- **Monitoring ready** with profiling and slow query logging

### 🛠️ Developer Experience
- **Docker Compose** for local development
- **Template-based configuration** for easy customization
- **Environment variable** driven setup
- **Comprehensive documentation** in every file
- **GitLab CI/CD pipeline** for automated deployments

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

| Tool | Minimum Version | Purpose |
|------|----------------|---------|
| **Docker** | 20.10+ | Container runtime |
| **Docker Compose** | 2.0+ | Container orchestration |
| **Git** | 2.0+ | Version control |

**Installation guides:**
- [Install Docker](https://docs.docker.com/get-docker/)
- [Install Docker Compose](https://docs.docker.com/compose/install/)

**System requirements:**
- **CPU:** 2+ cores recommended
- **RAM:** 2GB+ available memory
- **Disk:** 10GB+ free space for data
- **OS:** Linux (Ubuntu, Debian, CentOS, RHEL) or macOS

---

## 🚀 Quick Start

Get MongoDB up and running in 5 minutes!

### Step 1: Clone the Repository

```bash
git clone https://github.com/hacknitive/infra-mongodb.git
cd infra-mongodb
```

### Step 2: Create Configuration Files from Templates

All configuration files use `.template` or `.example` extensions. Create your working copies:

```bash
# Copy environment configuration
cp .env.example .env

# Copy Docker Compose configuration
cp docker-compose.yml.template docker-compose.yml

# Copy MongoDB server configuration
cp mongod.conf.template mongod.conf

# Copy Docker entrypoint script
cp docker-entrypoint.sh.template docker-entrypoint.sh

# Copy Dockerfile
cp Dockerfile.template Dockerfile

# Optional: Copy GitLab CI/CD pipeline (if using GitLab)
cp .gitlab-ci.yml.template .gitlab-ci.yml
```

### Step 3: Configure Your Environment

**Edit the `.env` file** and update these critical settings:

```bash
# Open with your favorite editor
nano .env
# or
vim .env
```

**Minimum required changes:**

```bash
# Set a strong root username (change from default)
MONGO_INITDB_ROOT_USERNAME=your_admin_username

# Set a strong root password (REQUIRED - change this!)
MONGO_INITDB_ROOT_PASSWORD=your_very_strong_password_here

# Optional: Change the port if 26444 is already in use
MONGODB_PORT=26444

# Optional: Customize container name
CONTAINER_NAME=my-mongodb-container
```

> 💡 **Pro Tip:** Generate a secure password using:
> ```bash
> openssl rand -base64 32
> ```

### Step 4: Launch MongoDB

```bash
# Start MongoDB in detached mode
docker compose up -d
```

**Expected output:**
```
[+] Running 3/3
 ✔ Network mongodb_default           Created
 ✔ Volume "mongodb-data"              Created
 ✔ Volume "mongodb-logs"              Created
 ✔ Container my-mongodb-container     Started
```

### Step 5: Verify Deployment

```bash
# Check container status (should show "healthy")
docker compose ps

# View logs
docker compose logs -f

# Test connection
docker compose exec mongodb mongosh --port 26444 --eval "db.hello()"
```

**Success indicators:**
- ✅ Container status shows `healthy`
- ✅ Logs show "Waiting for connections on port 26444"
- ✅ `db.hello()` command returns MongoDB server information

---

## ⚙️ Configuration Guide

### Environment Variables

The `.env` file contains 40+ configuration options organized into sections:

#### 🔐 Authentication & Security
```bash
MONGO_INITDB_ROOT_USERNAME=root          # Root username
MONGO_INITDB_ROOT_PASSWORD=changeme      # Root password (CHANGE THIS!)
MONGODB_AUTHORIZATION=enabled            # Enable/disable auth
```

#### 🌐 Network Configuration
```bash
MONGODB_PORT=26444                       # External port
MONGODB_BIND_IP=0.0.0.0                 # Bind address (0.0.0.0 = all interfaces)
MONGODB_MAX_CONNECTIONS=100              # Max simultaneous connections
```

#### 💾 Storage & Performance
```bash
WIREDTIGER_CACHE_SIZE=1                  # Cache size in GB (25-50% of RAM)
MONGODB_JOURNAL_COMPRESSOR=snappy        # Journal compression (snappy/zstd)
MONGODB_BLOCK_COMPRESSOR=snappy          # Data compression (snappy/zstd)
MONGODB_COMPRESSION_COMPRESSORS=snappy,zstd  # Network compression
```

#### 🖥️ Resource Limits
```bash
MONGODB_CPUS=2                           # Number of CPU cores
MONGODB_CPUSET_CPUS=0-1                  # Specific CPU cores to use
MONGODB_MEMORY=2g                        # Memory limit
MONGODB_MEMORY_SWAP=2g                   # Memory + swap limit
MONGODB_PIDS_LIMIT=2048                  # Max processes/threads
```

#### 📝 Logging & Monitoring
```bash
MONGODB_LOG_VERBOSITY=0                  # Log level (0-5)
MONGODB_OPERATION_PROFILING_MODE=1       # Profiling (0=off, 1=slow, 2=all)
MONGODB_SLOW_QUERY_MS=100                # Slow query threshold (ms)
MONGODB_LOG_MAX_SIZE=10m                 # Log rotation size
MONGODB_LOG_MAX_FILE=3                   # Number of log files to keep
```

### MongoDB Configuration

The `mongod.conf` file uses YAML format with environment variable substitution:

```yaml
storage:
  dbPath: /data/db
  wiredTiger:
    engineConfig:
      cacheSizeGB: ${WIREDTIGER_CACHE_SIZE}

systemLog:
  verbosity: ${MONGODB_LOG_VERBOSITY}

net:
  port: ${MONGODB_PORT}
  bindIp: "${MONGODB_BIND_IP}"

security:
  authorization: "${MONGODB_AUTHORIZATION}"
```

> 📖 **Full documentation** is available in each template file with detailed comments.

---

## 🎨 Deployment Options

### Option 1: Docker Compose (Recommended for Local/Dev)

```bash
# Start service
docker compose up -d

# Stop service
docker compose down

# View logs
docker compose logs -f mongodb

# Restart service
docker compose restart mongodb
```

### Option 2: Docker CLI (Manual Deployment)

```bash
# Build image
docker build -t mongodb:7.0-optimized .

# Run container
docker run -d \
  --name my-mongodb \
  --restart unless-stopped \
  -p 26444:26444 \
  --env-file .env \
  -v mongodb-data:/data/db \
  -v mongodb-logs:/var/log/mongodb \
  --cpus="2" \
  --memory="2g" \
  mongodb:7.0-optimized
```

### Option 3: GitLab CI/CD (Automated Deployment)

The repository includes a `.gitlab-ci.yml.template` for automated deployments:

1. Copy the template: `cp .gitlab-ci.yml.template .gitlab-ci.yml`
2. Configure GitLab CI/CD variables (see [CI/CD Integration](#-cicd-integration))
3. Push to your repository
4. Manually trigger deployment from GitLab UI

**Supported environments:**
- `develop` branch → Development environment
- `staging` branch → Staging environment

---

## 🔌 Connecting to MongoDB

### Connection String Format

```
mongodb://<username>:<password>@<host>:<port>/<database>?authSource=admin
```

### Root User Connection (Admin Access)

```bash
# Connection string
mongodb://root:your_password@localhost:26444/admin?authSource=admin

# Using mongosh CLI
mongosh "mongodb://root:your_password@localhost:26444/admin"

# Using Docker exec
docker compose exec mongodb mongosh --username root --password your_password --port 26444
```

### Application Connection Examples

#### Node.js (Mongoose)
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://root:your_password@localhost:26444/myapp?authSource=admin', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```

#### Python (PyMongo)
```python
from pymongo import MongoClient

client = MongoClient('mongodb://root:your_password@localhost:26444/myapp?authSource=admin')
db = client.myapp
```

#### Java (MongoDB Driver)
```java
MongoClient mongoClient = MongoClients.create(
    "mongodb://root:your_password@localhost:26444/myapp?authSource=admin"
);
```

#### Connection from Another Container

```yaml
services:
  app:
    image: myapp:latest
    environment:
      - MONGODB_URI=mongodb://root:password@mongodb:26444/myapp?authSource=admin
    depends_on:
      - mongodb
```

---

## 🎮 Management Commands

### Container Management

```bash
# Start MongoDB
docker compose up -d

# Stop MongoDB
docker compose down

# Restart MongoDB
docker compose restart

# View real-time logs
docker compose logs -f

# Execute commands inside container
docker compose exec mongodb bash

# Check container health
docker compose ps
```

### Data Management

```bash
# Create a backup
docker compose exec mongodb mongodump --out /data/backup --port 26444 --username root --password your_password

# Restore from backup
docker compose exec mongodb mongorestore /data/backup --port 26444 --username root --password your_password

# Copy backup to host
docker cp mongodb-container:/data/backup ./backup

# Remove all data (DESTRUCTIVE!)
docker compose down -v
```

### Monitoring & Debugging

```bash
# Monitor resource usage
docker stats mongodb-container

# View MongoDB server status
docker compose exec mongodb mongosh --port 26444 --eval "db.serverStatus()" --username root --password your_password

# Check current operations
docker compose exec mongodb mongosh --port 26444 --eval "db.currentOp()" --username root --password your_password

# View database statistics
docker compose exec mongodb mongosh --port 26444 --eval "db.stats()" --username root --password your_password
```

---

## 🔄 CI/CD Integration

### GitLab CI/CD Setup

1. **Copy the pipeline template:**
   ```bash
   cp .gitlab-ci.yml.template .gitlab-ci.yml
   ```

2. **Configure GitLab CI/CD Variables:**
   
   Navigate to: **Settings** → **CI/CD** → **Variables**
   
   Add the following variable:
   
   | Variable | Value | Protected | Masked |
   |----------|-------|-----------|--------|
   | `ENV_FILE_PATH` | `/path/to/.env` | ✅ | ❌ |

3. **Upload `.env` file to GitLab:**
   
   **Settings** → **CI/CD** → **Variables** → **Add Variable** → **File**
   
   - Key: `ENV_FILE`
   - Value: (paste contents of your `.env` file)
   - Protected: ✅
   - Masked: ❌

4. **Configure GitLab Runners:**
   
   Ensure runners are tagged properly:
   - Development runner: `development-1`
   - Staging runner: `staging1`

5. **Trigger Deployment:**
   
   Push to `develop` or `staging` branch, then manually trigger the pipeline from GitLab UI.

### Pipeline Features

- ✅ **Automated Docker image building**
- ✅ **Health check verification**
- ✅ **Multi-environment support**
- ✅ **Resource constraint enforcement**
- ✅ **Security capability management**
- ✅ **Rollback capability** (stop old, start new)

---

## ⚡ Performance Tuning

### WiredTiger Cache Size

**Rule of thumb:** 25-50% of available RAM

```bash
# For a system with 8GB RAM
WIREDTIGER_CACHE_SIZE=2  # 2GB cache

# For a system with 16GB RAM
WIREDTIGER_CACHE_SIZE=6  # 6GB cache
```

### Connection Pool Sizing

```bash
# Light usage (< 100 concurrent users)
MONGODB_MAX_CONNECTIONS=100

# Medium usage (100-500 concurrent users)
MONGODB_MAX_CONNECTIONS=500

# Heavy usage (500+ concurrent users)
MONGODB_MAX_CONNECTIONS=1000
```

### Journal Commit Interval

**Trade-off:** Durability vs. Performance

```bash
# Maximum durability (slower writes)
MONGODB_JOURNAL_COMMIT_INTERVAL_MS=50

# Balanced (recommended)
MONGODB_JOURNAL_COMMIT_INTERVAL_MS=100

# Maximum performance (risk of data loss)
MONGODB_JOURNAL_COMMIT_INTERVAL_MS=300
```

### Compression Settings

```bash
# Best performance (lower compression ratio)
MONGODB_JOURNAL_COMPRESSOR=snappy
MONGODB_BLOCK_COMPRESSOR=snappy

# Best compression (higher CPU usage)
MONGODB_JOURNAL_COMPRESSOR=zstd
MONGODB_BLOCK_COMPRESSOR=zstd
```

---

## 🔒 Security Best Practices

### 1. Strong Credentials

```bash
# ❌ DON'T use default or weak passwords
MONGO_INITDB_ROOT_PASSWORD=root123

# ✅ DO use strong, randomly generated passwords
MONGO_INITDB_ROOT_PASSWORD=$(openssl rand -base64 32)
```

### 2. Network Isolation

```bash
# ❌ DON'T expose to all interfaces in production
MONGODB_BIND_IP=0.0.0.0

# ✅ DO bind to localhost only
MONGODB_BIND_IP=127.0.0.1

# ✅ OR use specific internal IP
MONGODB_BIND_IP=10.0.0.5
```

### 3. Enable Authentication

```bash
# ✅ Always enable authentication
MONGODB_AUTHORIZATION=enabled
```

### 4. Use Secrets Management

For production, use Docker secrets or environment secret managers:

```yaml
# docker-compose.yml with secrets
services:
  mongodb:
    secrets:
      - mongodb_root_password
secrets:
  mongodb_root_password:
    file: ./secrets/mongodb_root_password.txt
```

### 5. Regular Updates

```bash
# Update to latest MongoDB 7.0 patch
docker compose pull
docker compose up -d
```

### 6. Firewall Configuration

```bash
# Only allow specific IPs to access MongoDB port
sudo ufw allow from 10.0.0.0/24 to any port 26444
sudo ufw deny 26444
```

---

## 🐛 Troubleshooting

### Container Won't Start

**Symptom:** Container exits immediately

```bash
# Check logs for errors
docker compose logs

# Common issues:
# 1. YAML syntax error in mongod.conf
# 2. Port already in use
# 3. Permission issues on volumes
```

**Solutions:**
```bash
# Fix permission issues
sudo chown -R 999:999 ./data ./logs

# Change port if in use
# Edit .env and change MONGODB_PORT
```

### Health Check Failing

**Symptom:** Container status shows "unhealthy"

```bash
# View health check logs
docker inspect mongodb-container | jq '.[0].State.Health'

# Common causes:
# 1. MongoDB hasn't finished starting
# 2. Port mismatch in health check
# 3. Configuration error
```

**Solutions:**
```bash
# Wait 30-60 seconds for startup
# Check MongoDB logs
docker compose logs -f

# Verify mongod is running
docker compose exec mongodb ps aux | grep mongod
```

### Connection Refused

**Symptom:** Cannot connect from application

```bash
# Check if MongoDB is listening
docker compose exec mongodb netstat -tlnp | grep 26444

# Check if port is exposed
docker compose ps
```

**Solutions:**
```bash
# Ensure MONGODB_BIND_IP allows connections
MONGODB_BIND_IP=0.0.0.0  # For Docker network access

# Verify port mapping
docker compose port mongodb 26444
```

### Performance Issues

**Symptom:** Slow queries, high CPU usage

```bash
# Check current operations
docker compose exec mongodb mongosh --eval "db.currentOp(true)"

# Enable profiling
docker compose exec mongodb mongosh --eval "db.setProfilingLevel(2)"

# Check slow queries
docker compose exec mongodb mongosh --eval "db.system.profile.find().limit(10).sort({ts:-1}).pretty()"
```

**Solutions:**
```bash
# Increase cache size
WIREDTIGER_CACHE_SIZE=4  # Adjust based on available RAM

# Add appropriate indexes
# Review query patterns and create indexes
```

### Disk Space Issues

```bash
# Check disk usage
docker compose exec mongodb df -h

# Check database sizes
docker compose exec mongodb mongosh --eval "db.stats()"

# Clean up old logs
docker compose exec mongodb find /var/log/mongodb -name "*.log.*" -delete
```

---

## 📁 Project Structure

```
infra-mongodb/
├── .env.example                    # Environment variables template
├── .gitignore                      # Git ignore rules
├── .gitlab-ci.yml.template         # GitLab CI/CD pipeline template
├── docker-compose.yml.template     # Docker Compose configuration template
├── Dockerfile.template             # Docker image build instructions template
├── docker-entrypoint.sh.template   # Container initialization script template
├── mongod.conf.template            # MongoDB server configuration template
├── README.md                       # This comprehensive guide
├── assets/
│   └── avatar.jpeg                 # Project logo/avatar
├── data/                           # MongoDB data directory (created on first run)
│   └── (MongoDB database files)
└── logs/                           # MongoDB log directory (created on first run)
    └── (MongoDB log files)
```

### File Descriptions

| File | Purpose |
|------|---------|
| `.env.example` | Template for environment variables with all configuration options |
| `docker-compose.yml.template` | Orchestrates the MongoDB container with all settings |
| `Dockerfile.template` | Builds optimized MongoDB image with custom configurations |
| `docker-entrypoint.sh.template` | Handles container initialization and configuration generation |
| `mongod.conf.template` | MongoDB server configuration with environment variable support |
| `.gitlab-ci.yml.template` | Automated CI/CD pipeline for GitLab deployments |
| `.gitignore` | Prevents committing sensitive files (.env, data/, logs/) |

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Reporting Issues

1. Check [existing issues](https://github.com/hacknitive/infra-mongodb/issues)
2. Create a new issue with:
   - Clear description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Docker version, etc.)

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes
4. Test thoroughly
5. Commit with clear messages: `git commit -m 'Add amazing feature'`
6. Push to your fork: `git push origin feature/amazing-feature`
7. Open a Pull Request

### Development Guidelines

- ✅ Add comments to new configuration options
- ✅ Update README.md if adding features
- ✅ Test with both Docker Compose and GitLab CI/CD
- ✅ Follow existing code style and structure
- ✅ Keep security in mind

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 Hacknitive

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🌟 Acknowledgments

- **MongoDB Team** - For the excellent database system
- **Docker Team** - For containerization technology
- **Community Contributors** - For feedback and improvements

---

## 📞 Support

- **GitHub Issues:** [https://github.com/hacknitive/infra-mongodb/issues](https://github.com/hacknitive/infra-mongodb/issues)
- **Documentation:** [MongoDB Official Docs](https://docs.mongodb.com/)
- **Docker Documentation:** [Docker Official Docs](https://docs.docker.com/)

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/hacknitive">Hacknitive</a>
</p>

<p align="center">
  <sub>⭐ Star this repository if you find it helpful!</sub>
</p>