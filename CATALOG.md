# 📚 Moien Delivery - Service Catalog | كتالوج الخدمات

<div align="center">

[![Services](https://img.shields.io/badge/Services-22-blue.svg)]()
[![Modules](https://img.shields.io/badge/Modules-11-green.svg)]()
[![APIs](https://img.shields.io/badge/APIs-RESTful-green.svg)]()
[![Status](https://img.shields.io/badge/Status-Active-success.svg)]()
[![Version](https://img.shields.io/badge/Version-v0.0.13-orange.svg)]()

</div>

- [مبادئ الكتالوج](#-مبادئ-الكتالوج)
- [العناوين](#-العناوين)
- [الوحدات (Modules)](#-الوحدات-modules)
- [الوقت الحقيقي (Realtime)](#-الوقت-الحقيقي-realtime)

---

## 🧭 مبادئ الكتالوج

- هذا الملف يوثق **ما هو موجود فعلاً** في الريبو من وحدات وخدمات.
- مصدر الحقيقة للـ endpoints وتفاصيل الطلب/الاستجابة هو: [docs/API.md](docs/API.md).
- لأحداث الوقت الحقيقي (Socket.IO): [docs/WEBSOCKETS.md](docs/WEBSOCKETS.md).

---

## 🌐 العناوين

- Backend Dashboard: `http://localhost:3000/`
- API Base URL: `http://localhost:3000/api/v1`
- Web Frontend: `http://localhost:4200/`

---

## 🧩 الوحدات (Modules)

| الوحدة      | Base route (API)                 | ملاحظات                                                         |
| ----------- | -------------------------------- | --------------------------------------------------------------- |
| Admin Staff | `/admin-staff`                   | CRUD أساسي + تسجيل دخول أدمن legacy (يوجد أيضاً JWT admin auth) |
| `GET`       | `/admin/reports/revenue`         | تقارير الإيرادات                                                |
| `POST`      | `/admin/promotions`              | إنشاء عرض                                                       |
| `POST`      | `/admin/notifications/broadcast` | إشعار جماعي                                                     |

---

## 📊 مقاييس الخدمات

### SLA Targets

| الخدمة       | التوفر | زمن الاستجابة |
| ------------ | ------ | ------------- |
| Auth         | 99.99% | < 100ms       |
| User         | 99.9%  | < 150ms       |
| Restaurant   | 99.9%  | < 200ms       |
| Order        | 99.95% | < 150ms       |
| Payment      | 99.99% | < 500ms       |
| Delivery     | 99.9%  | < 100ms       |
| Notification | 99.9%  | < 200ms       |

---

<div align="center">

**آخر تحديث: فبراير 2026**

[🔙 العودة للرئيسية](README.md) | [🏗️ الهيكل المعماري](ARCHITECTURE.md)

</div>
