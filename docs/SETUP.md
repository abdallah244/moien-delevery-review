# 🛠️ دليل الإعداد والتثبيت | Setup Guide

<div align="center">

[![Setup](https://img.shields.io/badge/Setup-Complete-success.svg)]()
[![Time](https://img.shields.io/badge/Time-15%20min-blue.svg)]()

**دليل شامل لإعداد بيئة التطوير**

</div>

---

## 📋 جدول المحتويات

- [المتطلبات الأساسية](#-المتطلبات-الأساسية)
- [إعداد الخادم الخلفي](#-إعداد-الخادم-الخلفي-backend)
- [إعداد واجهة الويب](#-إعداد-واجهة-الويب-web)
- [إعداد تطبيقات الموبايل](#-إعداد-تطبيقات-الموبايل-mobile)
- [إعداد قاعدة البيانات](#-إعداد-قاعدة-البيانات)
- [إعداد Docker](#-إعداد-docker-اختياري)
- [متغيرات البيئة](#-متغيرات-البيئة)
- [استكشاف الأخطاء](#-استكشاف-الأخطاء)

---

## 📦 المتطلبات الأساسية

### البرامج المطلوبة

| البرنامج   | الإصدار | التحميل                                               |
| ---------- | ------- | ----------------------------------------------------- |
| Node.js    | 20.0+   | [تحميل](https://nodejs.org/)                          |
| npm        | 10.0+   | يأتي مع Node.js                                       |
| Flutter    | 3.10+   | [تحميل](https://flutter.dev/docs/get-started/install) |
| PostgreSQL | 15.0+   | [تحميل](https://www.postgresql.org/download/)         |
| Redis      | 7.0+    | [تحميل](https://redis.io/download/)                   |
| Git        | 2.40+   | [تحميل](https://git-scm.com/downloads)                |
| Docker     | 24.0+   | [تحميل](https://www.docker.com/get-started) (اختياري) |

### التحقق من التثبيت

```bash
# Node.js
node --version
# Expected: v20.x.x

# npm
npm --version
# Expected: 10.x.x

# Flutter
flutter --version
# Expected: Flutter 3.10.x

# PostgreSQL
psql --version
# Expected: psql (PostgreSQL) 15.x

# Redis
redis-server --version
# Expected: Redis server v=7.x.x

# Git
git --version
# Expected: git version 2.40.x
```

---

## 💻 إعداد الخادم الخلفي (Backend)

### 1. استنساخ المشروع

```bash
git clone https://github.com/moien-delivery/moien-delivery.git
cd moien-delivery
```

### 2. الانتقال لمجلد الخادم

```bash
cd backend
```

### 3. تثبيت التبعيات

```bash
npm install
```

### 4. إعداد ملف البيئة

```bash
# نسخ ملف البيئة
cp .env.example .env

# تعديل الإعدادات
nano .env
# أو
code .env
```

### 5. محتوى ملف .env

```env
# Application
NODE_ENV=development
PORT=3000
API_PREFIX=api/v1

# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=moien_delivery
DATABASE_USER=postgres
DATABASE_PASSWORD=your_password

# Redis
REDIS_URL=
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_TLS=false

# Cache
CACHE_TTL_MS=60000
CACHE_MAX_ITEMS=10000

# Performance / HTTP
COMPRESSION_ENABLED=true
COMPRESSION_THRESHOLD_BYTES=1024
UPLOADS_CACHE_MAXAGE_SECONDS=3600

# Database Pooling (pg)
DB_POOL_MAX=20
DB_POOL_IDLE_TIMEOUT_MS=30000
DB_POOL_CONNECTION_TIMEOUT_MS=2000

# JWT
JWT_SECRET=your_super_secret_key_here
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=your_refresh_secret_key
JWT_REFRESH_EXPIRES_IN=7d

# Mail (SMTP)
# Recommended: Gmail SMTP + App Password
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_SECURE=false
MAIL_USER=your_gmail@gmail.com
MAIL_PASS=your_gmail_app_password
# Optional: if empty, backend can fallback to `Moien Delivery <MAIL_USER>`
MAIL_FROM=
MAIL_REPLY_TO=your_gmail@gmail.com

# SMS (Twilio)
TWILIO_ACCOUNT_SID=your_sid
TWILIO_AUTH_TOKEN=your_token
TWILIO_PHONE_NUMBER=+1234567890

# WhatsApp (Twilio)
# Notes:
# - WhatsApp messages must be sent from a WhatsApp-enabled sender in Twilio.
# - For testing, use the Twilio WhatsApp Sandbox sender and have recipients join the sandbox.
TWILIO_WHATSAPP_ENABLED=true
# Can be either "whatsapp:+<number>" or "+<number>". If empty, backend falls back to TWILIO_PHONE_NUMBER.
TWILIO_WHATSAPP_FROM=
# Optional: if you store phone numbers locally like 011..., set a default calling code (Egypt = 20)
TWILIO_DEFAULT_COUNTRY_CALLING_CODE=20

# Payment (Stripe)
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# Google Maps
GOOGLE_MAPS_API_KEY=your_api_key

# Firebase
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_PRIVATE_KEY=your_private_key
FIREBASE_CLIENT_EMAIL=your_client_email

# Storage (AWS S3)
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=eu-central-1
AWS_S3_BUCKET=moien-delivery

# Storage (Cloudinary)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
# Optional
CLOUDINARY_FOLDER=moien/admin-staff

# Logging
LOG_LEVEL=debug
```

### 6. إعداد قاعدة البيانات

```bash
# تشغيل الهجرات (TypeORM)
npm run db:migrate:run

# تشغيل البذور (بيانات تجريبية)
npm run db:seed
```

### 7. تشغيل الخادم

```bash
# وضع التطوير
npm run start:dev

# وضع الإنتاج
npm run build
npm run start:prod
```

### 8. التحقق

افتح المتصفح على: `http://localhost:3000`

يجب أن ترى **لوحة تحكم الخادم** مع:

- ✅ حالة الخادم: **Online**
- 📊 معلومات: الإصدار، البيئة، المنفذ، الوقت
- 🔗 روابط الـ Endpoints المتاحة

#### Endpoints المتاحة (v0.0.1)

| المسار                                       | الوصف            | النوع |
| -------------------------------------------- | ---------------- | ----- |
| `http://localhost:3000`                      | لوحة تحكم الخادم | HTML  |
| `http://localhost:3000/api/v1/health`        | فحص صحة الخادم   | JSON  |
| `http://localhost:3000/api/v1/health/status` | صفحة حالة Health | HTML  |
| `http://localhost:3000/api/docs`             | صفحة التوثيق     | HTML  |

#### Health Check Response

```json
{
  "status": "ok",
  "timestamp": "2026-02-01T12:00:00.000Z",
  "uptime": 3600.123
}
```

---

## 🌐 إعداد واجهة الويب (Web)

### 1. الانتقال لمجلد الويب

```bash
cd wfrontend
```

### 2. تثبيت التبعيات

```bash
npm install
```

### 3. إعداد ملف البيئة

```bash
# إنشاء ملف البيئة
cp src/environments/environment.example.ts src/environments/environment.ts
```

### 4. محتوى ملف environment.ts

```typescript
export const environment = {
  production: false,
  apiUrl: "http://localhost:3000/api/v1",
  googleMapsApiKey: "your_api_key",
  stripePublishableKey: "pk_test_xxx",
  firebaseConfig: {
    apiKey: "your_api_key",
    authDomain: "your_project.firebaseapp.com",
    projectId: "your_project_id",
    storageBucket: "your_project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your_app_id",
  },
};
```

### 5. تشغيل التطبيق

```bash
# وضع التطوير
npm start

# أو
ng serve
```

### 6. التحقق

افتح المتصفح على: `http://localhost:4200`

---

## 📱 إعداد تطبيقات الموبايل (Mobile)

### 1. الانتقال لمجلد الموبايل

```bash
cd mfrontend
```

### 2. تثبيت تبعيات Flutter

```bash
flutter pub get
```

### 3. إعداد ملف التكوين

```bash
# إنشاء ملف التكوين
cp lib/core/config/app_config.example.dart lib/core/config/app_config.dart
```

### 4. محتوى app_config.dart

```dart
class AppConfig {
  static const String apiBaseUrl = 'http://10.0.2.2:3000/api/v1'; // للمحاكي
  // static const String apiBaseUrl = 'http://localhost:3000/api/v1'; // للويب

  static const String googleMapsApiKey = 'your_api_key';
  static const String stripePublishableKey = 'pk_test_xxx';

  static const firebaseConfig = {
    'apiKey': 'your_api_key',
    'projectId': 'your_project_id',
  };
}
```

### 5. إعداد Android

```bash
# الانتقال لمجلد Android
cd android

# إنشاء ملف local.properties (إذا لم يكن موجوداً)
echo "sdk.dir=/path/to/android/sdk" > local.properties
echo "flutter.sdk=/path/to/flutter" >> local.properties
```

### 6. إعداد iOS

```bash
cd ios
pod install
```

### 7. تشغيل التطبيق

```bash
# قائمة الأجهزة المتاحة
flutter devices

# تشغيل على جهاز محدد
flutter run -d <device_id>

# تشغيل على Android
flutter run -d android

# تشغيل على iOS
flutter run -d ios

# تشغيل على المتصفح
flutter run -d chrome
```

---

## 🗄️ إعداد قاعدة البيانات

### PostgreSQL

```bash
# تشغيل PostgreSQL
# على macOS (Homebrew)
brew services start postgresql@15

# على Ubuntu
sudo systemctl start postgresql

# على Windows
# من Services أو PostgreSQL Manager
```

### إنشاء قاعدة البيانات

```bash
# الدخول لـ PostgreSQL
psql -U postgres

# إنشاء قاعدة البيانات
CREATE DATABASE moien_delivery;

# إنشاء مستخدم (اختياري)
CREATE USER moien_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE moien_delivery TO moien_user;

# الخروج
\q
```

### Redis

```bash
# تشغيل Redis
# على macOS (Homebrew)
brew services start redis

# على Ubuntu
sudo systemctl start redis

# على Windows
# استخدم Windows Subsystem for Linux (WSL)
redis-server
```

---

## 🐳 إعداد Docker (اختياري)

### docker-compose.yml

```yaml
version: "3.8"

services:
  # PostgreSQL
  postgres:
    image: postgres:15-alpine
    container_name: moien_postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: moien_delivery
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  # Redis
  redis:
    image: redis:7-alpine
    container_name: moien_redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  # Backend API
  api:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: moien_api
    environment:
      - NODE_ENV=development
      - DATABASE_HOST=postgres
      - REDIS_HOST=redis
    ports:
      - "3000:3000"
    depends_on:
      - postgres
      - redis
    volumes:
      - ./backend:/app
      - /app/node_modules

  # Web Frontend
  web:
    build:
      context: ./wfrontend
      dockerfile: Dockerfile
    container_name: moien_web
    ports:
      - "4200:4200"
    volumes:
      - ./wfrontend:/app
      - /app/node_modules
    depends_on:
      - api

volumes:
  postgres_data:
  redis_data:
```

### تشغيل Docker

```bash
# بناء وتشغيل جميع الخدمات
docker-compose up -d

# عرض الحالة
docker-compose ps

# عرض السجلات
docker-compose logs -f

# إيقاف الخدمات
docker-compose down
```

---

## ⚙️ متغيرات البيئة

### ملخص المتغيرات المطلوبة

| المتغير         | الوصف                | مطلوب |
| --------------- | -------------------- | ----- |
| `NODE_ENV`      | بيئة التشغيل         | ✅    |
| `DATABASE_*`    | إعدادات PostgreSQL   | ✅    |
| `REDIS_*`       | إعدادات Redis        | ✅    |
| `JWT_*`         | إعدادات المصادقة     | ✅    |
| `MAIL_*`        | إعدادات البريد       | ❌    |
| `TWILIO_*`      | إعدادات SMS/WhatsApp | ❌    |
| `STRIPE_*`      | إعدادات الدفع        | ❌    |
| `GOOGLE_MAPS_*` | إعدادات الخرائط      | ❌    |
| `FIREBASE_*`    | إعدادات الإشعارات    | ❌    |
| `AWS_*`         | إعدادات التخزين      | ❌    |

---

## 🔧 استكشاف الأخطاء

### مشاكل Node.js

```bash
# مسح الكاش
npm cache clean --force

# حذف node_modules وإعادة التثبيت
rm -rf node_modules package-lock.json
npm install
```

### مشاكل Flutter

```bash
# تنظيف المشروع
flutter clean

# تحديث التبعيات
flutter pub upgrade

# تشخيص المشاكل
flutter doctor -v
```

### مشاكل قاعدة البيانات

```bash
# التحقق من اتصال PostgreSQL
psql -U postgres -h localhost -p 5432

# إعادة تشغيل الخدمة
sudo systemctl restart postgresql
```

### مشاكل Redis

```bash
# التحقق من Redis
redis-cli ping
# Expected: PONG

# عرض المعلومات
redis-cli info
```

---

## 📞 الدعم

إذا واجهت أي مشاكل:

1. 📖 راجع [FAQ](FAQ.md)
2. 🔍 ابحث في [Issues](https://github.com/moien-delivery/moien-delivery/issues)
3. 💬 اسأل في [Discord](https://discord.gg/moiendelivery)
4. 📧 راسلنا على [support@moiendelivery.com](mailto:support@moiendelivery.com)

---

<div align="center">

**🎉 تهانينا! أنت جاهز للبدء**

[🔙 العودة للتوثيق](README.md) | [🚀 البداية السريعة](QUICKSTART.md)

</div>
