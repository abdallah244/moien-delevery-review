# 🚀 دليل النشر | Deployment Guide

<div align="center">

[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)]()
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Ready-green.svg)]()
[![CI/CD](https://img.shields.io/badge/CI/CD-GitHub%20Actions-orange.svg)]()

**دليل شامل لنشر Moien Delivery**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [المتطلبات](#-المتطلبات)
- [النشر بـ Docker](#-النشر-بـ-docker)
- [النشر بـ Kubernetes](#-النشر-بـ-kubernetes)
- [CI/CD](#-cicd)
- [متغيرات الإنتاج](#-متغيرات-الإنتاج)
- [SSL/TLS](#-ssltls)
- [المراقبة](#-المراقبة)
- [النسخ الاحتياطي](#-النسخ-الاحتياطي)

---

## 🎯 نظرة عامة

### بنية النشر

```
                         ┌─────────────────┐
                         │   CloudFlare    │
                         │   (CDN + WAF)   │
                         └────────┬────────┘
                                  │
                         ┌────────▼────────┐
                         │  Load Balancer  │
                         │    (NGINX)      │
                         └────────┬────────┘
                                  │
           ┌──────────────────────┼──────────────────────┐
           │                      │                      │
    ┌──────▼──────┐       ┌──────▼──────┐       ┌──────▼──────┐
    │   API Pod   │       │   API Pod   │       │   API Pod   │
    │  (NestJS)   │       │  (NestJS)   │       │  (NestJS)   │
    └──────┬──────┘       └──────┬──────┘       └──────┬──────┘
           │                      │                      │
           └──────────────────────┼──────────────────────┘
                                  │
           ┌──────────────────────┼──────────────────────┐
           │                      │                      │
    ┌──────▼──────┐       ┌──────▼──────┐       ┌──────▼──────┐
    │ PostgreSQL  │       │    Redis    │       │     S3      │
    │  (Primary)  │       │  (Cluster)  │       │  (Storage)  │
    └─────────────┘       └─────────────┘       └─────────────┘
```

---

## 📦 المتطلبات

### الخوادم

| الخدمة     | المواصفات الدنيا | المواصفات الموصى بها |
| ---------- | ---------------- | -------------------- |
| API Server | 2 vCPU, 4GB RAM  | 4 vCPU, 8GB RAM      |
| PostgreSQL | 2 vCPU, 4GB RAM  | 4 vCPU, 16GB RAM     |
| Redis      | 1 vCPU, 2GB RAM  | 2 vCPU, 4GB RAM      |

### البرامج

- Docker 24.0+
- Docker Compose 2.20+
- Kubernetes 1.28+ (اختياري)
- Helm 3.12+ (اختياري)

---

## 🐳 النشر بـ Docker

### docker-compose.prod.yml

```yaml
version: "3.8"

services:
  # API Server
  api:
    image: moiendelivery/api:latest
    container_name: moien_api
    restart: always
    environment:
      - NODE_ENV=production
      - DATABASE_HOST=postgres
      - REDIS_HOST=redis
    ports:
      - "3000:3000"
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/api/v1/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: "1"
          memory: 1G
        reservations:
          cpus: "0.5"
          memory: 512M

  # Web Frontend
  web:
    image: moiendelivery/web:latest
    container_name: moien_web
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/ssl/certs
    depends_on:
      - api

  # PostgreSQL
  postgres:
    image: postgres:15-alpine
    container_name: moien_postgres
    restart: always
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./backups:/backups
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis
  redis:
    image: redis:7-alpine
    container_name: moien_redis
    restart: always
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  # NGINX Load Balancer
  nginx:
    image: nginx:alpine
    container_name: moien_nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/ssl/certs
    depends_on:
      - api
      - web

volumes:
  postgres_data:
  redis_data:

networks:
  default:
    name: moien_network
```

### NGINX Configuration

```nginx
# nginx/nginx.conf
upstream api_servers {
    least_conn;
    server api:3000 weight=1 max_fails=3 fail_timeout=30s;
}

server {
    listen 80;
    server_name api.moiendelivery.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name api.moiendelivery.com;

    ssl_certificate /etc/ssl/certs/fullchain.pem;
    ssl_certificate_key /etc/ssl/certs/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;

    location / {
        proxy_pass http://api_servers;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
        proxy_read_timeout 90;
    }

    # Rate Limiting
    limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
    limit_req zone=api burst=20 nodelay;

    # Security Headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
}
```

### أوامر النشر

```bash
# بناء الصور
docker-compose -f docker-compose.prod.yml build

# تشغيل الخدمات
docker-compose -f docker-compose.prod.yml up -d

# عرض الحالة
docker-compose -f docker-compose.prod.yml ps

# عرض السجلات
docker-compose -f docker-compose.prod.yml logs -f

# تحديث خدمة
docker-compose -f docker-compose.prod.yml up -d --no-deps --build api

# إيقاف الخدمات
docker-compose -f docker-compose.prod.yml down
```

---

## ☸️ النشر بـ Kubernetes

### Namespace

```yaml
# k8s/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: moien-delivery
```

### API Deployment

```yaml
# k8s/api-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: moien-api
  namespace: moien-delivery
spec:
  replicas: 3
  selector:
    matchLabels:
      app: moien-api
  template:
    metadata:
      labels:
        app: moien-api
    spec:
      containers:
        - name: api
          image: moiendelivery/api:latest
          ports:
            - containerPort: 3000
          env:
            - name: NODE_ENV
              value: "production"
            - name: DATABASE_HOST
              valueFrom:
                secretKeyRef:
                  name: moien-secrets
                  key: database-host
          resources:
            requests:
              memory: "512Mi"
              cpu: "250m"
            limits:
              memory: "1Gi"
              cpu: "500m"
          livenessProbe:
            httpGet:
              path: /api/v1/health
              port: 3000
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /api/v1/health
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: moien-api-service
  namespace: moien-delivery
spec:
  selector:
    app: moien-api
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP
```

### Ingress

```yaml
# k8s/ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: moien-ingress
  namespace: moien-delivery
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
    - hosts:
        - api.moiendelivery.com
      secretName: moien-tls
  rules:
    - host: api.moiendelivery.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: moien-api-service
                port:
                  number: 80
```

### أوامر Kubernetes

```bash
# تطبيق جميع الموارد
kubectl apply -f k8s/

# عرض الـ Pods
kubectl get pods -n moien-delivery

# عرض الخدمات
kubectl get services -n moien-delivery

# عرض السجلات
kubectl logs -f deployment/moien-api -n moien-delivery

# توسيع النطاق
kubectl scale deployment moien-api --replicas=5 -n moien-delivery
```

---

## 🔄 CI/CD

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: npm ci
        working-directory: ./backend

      - name: Run tests
        run: npm run test
        working-directory: ./backend

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t moiendelivery/api:${{ github.sha }} ./backend

      - name: Push to Registry
        run: |
          echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
          docker push moiendelivery/api:${{ github.sha }}

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Kubernetes
        uses: azure/k8s-deploy@v4
        with:
          manifests: |
            k8s/api-deployment.yaml
          images: |
            moiendelivery/api:${{ github.sha }}
```

---

## ⚙️ متغيرات الإنتاج

```env
# .env.production
NODE_ENV=production
PORT=3000

# Database
DATABASE_HOST=postgres.moiendelivery.com
DATABASE_PORT=5432
DATABASE_NAME=moien_production
DATABASE_USER=moien_user
DATABASE_PASSWORD=super_secure_password

# Redis
REDIS_HOST=redis.moiendelivery.com
REDIS_PORT=6379
REDIS_PASSWORD=redis_secure_password

# JWT
JWT_SECRET=production_jwt_secret_key_256_bits
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=production_refresh_secret_key
JWT_REFRESH_EXPIRES_IN=7d

# External Services
STRIPE_SECRET_KEY=sk_live_xxx
GOOGLE_MAPS_API_KEY=xxx
FIREBASE_PROJECT_ID=moien-delivery-prod
AWS_S3_BUCKET=moien-delivery-prod

# Cloudinary (Employee photos)
CLOUDINARY_CLOUD_NAME=prod_cloud_name
CLOUDINARY_API_KEY=prod_api_key
CLOUDINARY_API_SECRET=prod_api_secret
# Optional
CLOUDINARY_FOLDER=moien/admin-staff
```

---

## 🔒 SSL/TLS

### Let's Encrypt with Certbot

```bash
# تثبيت Certbot
apt-get install certbot python3-certbot-nginx

# الحصول على شهادة
certbot --nginx -d api.moiendelivery.com -d moiendelivery.com

# التجديد التلقائي
certbot renew --dry-run
```

---

## 📊 المراقبة

### Prometheus + Grafana

```yaml
# monitoring/prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "moien-api"
    static_configs:
      - targets: ["api:3000"]
```

---

## 💾 النسخ الاحتياطي

### PostgreSQL Backup

```bash
#!/bin/bash
# scripts/backup.sh

BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)
FILENAME="moien_backup_$DATE.sql.gz"

pg_dump -h postgres -U $DB_USER $DB_NAME | gzip > $BACKUP_DIR/$FILENAME

# رفع للـ S3
aws s3 cp $BACKUP_DIR/$FILENAME s3://moien-backups/database/

# حذف النسخ القديمة (أكثر من 7 أيام)
find $BACKUP_DIR -type f -mtime +7 -delete
```

### Cron Job

```bash
# كل يوم الساعة 3 صباحاً
0 3 * * * /scripts/backup.sh >> /var/log/backup.log 2>&1
```

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [🐳 Docker](DOCKER.md)

</div>
