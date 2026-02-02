# 🚀 البداية السريعة | Quickstart Guide

<div align="center">

[![Time](https://img.shields.io/badge/Time-5%20min-success.svg)]()

**ابدأ العمل على Moien Delivery في 5 دقائق**

</div>

---

## ⚡ الخطوات السريعة

### 1️⃣ استنساخ المشروع

```bash
git clone https://github.com/moien-delivery/moien-delivery.git
cd moien-delivery
```

### 2️⃣ تشغيل بـ Docker (الأسهل)

```bash
docker-compose up -d
```

انتظر دقيقة، ثم:

- 💻 لوحة التحكم: http://localhost:3000
- 📡 API: http://localhost:3000/api/v1
- 🟢 Health Check: http://localhost:3000/api/v1/health
- 🌐 الويب: http://localhost:4200

### 3️⃣ أو بدون Docker

```bash
# Terminal 1 - Backend
cd backend
npm install
npm run start:dev

# Terminal 2 - Web
cd wfrontend
npm install
npm start

# Terminal 3 - Mobile
cd mfrontend
flutter pub get
flutter run
```

---

## 🎉 هذا كل شيء!

للمزيد من التفاصيل، راجع [دليل الإعداد الكامل](SETUP.md).

---

<div align="center">

[🔙 العودة للتوثيق](README.md)

</div>
