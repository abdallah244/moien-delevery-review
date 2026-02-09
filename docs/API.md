# 📡 توثيق API | API Documentation

<div align="center">

[![API](https://img.shields.io/badge/API-RESTful-success.svg)]()
[![Version](https://img.shields.io/badge/Version-v1-blue.svg)]()
[![OpenAPI](https://img.shields.io/badge/OpenAPI-3.0-green.svg)]()

**التوثيق الكامل لـ Moien Delivery API**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [المصادقة](#-المصادقة)
- [نقاط النهاية](#-نقاط-النهاية)
- [رموز الاستجابة](#-رموز-الاستجابة)
- [معالجة الأخطاء](#-معالجة-الأخطاء)
- [التحديد (Rate Limiting)](#-التحديد-rate-limiting)
- [الصفحات (Pagination)](#-الصفحات-pagination)

---

## 🎯 نظرة عامة

### عنوان الـ API

| البيئة  | العنوان                            |
| ------- | ---------------------------------- |
| التطوير | `http://localhost:3000/api/v1`     |
| الإنتاج | `https://api.moiendelivery.com/v1` |

### ✅ Endpoints المتاحة حالياً (v0.0.13)

| المسار                  | الطريقة | الوصف                    | الحالة         |
| ----------------------- | ------- | ------------------------ | -------------- |
| `/`                     | GET     | لوحة تحكم الخادم (HTML)  | ✅ متاح        |
| `/api/v1/health`        | GET     | فحص صحة الخادم (JSON)    | ✅ متاح        |
| `/api/v1/health/status` | GET     | صفحة حالة Health (HTML)  | ✅ متاح        |
| `/api/docs`             | GET     | صفحة حالة التوثيق (HTML) | ⚠️ قيد التطوير |

#### 👥 Admin Staff Module

| المسار                      | الطريقة | الوصف                 | الحالة  |
| --------------------------- | ------- | --------------------- | ------- |
| `/api/v1/admin-staff`       | GET     | قائمة الموظفين        | ✅ متاح |
| `/api/v1/admin-staff`       | POST    | إنشاء موظف + رفع صورة | ✅ متاح |
| `/api/v1/admin-staff/login` | POST    | تسجيل دخول الأدمن     | ✅ متاح |
| `/api/v1/admin-staff/:id`   | DELETE  | حذف موظف              | ✅ متاح |

#### 👤 Users Module

| المسار                                   | الطريقة | الوصف                             | الحالة  |
| ---------------------------------------- | ------- | --------------------------------- | ------- |
| `/api/v1/users`                          | GET     | قائمة المستخدمين                  | ✅ متاح |
| `/api/v1/users/summary`                  | GET     | ملخص المستخدمين                   | ✅ متاح |
| `/api/v1/users/register`                 | POST    | تسجيل مستخدم جديد                 | ✅ متاح |
| `/api/v1/users/login`                    | POST    | تسجيل دخول (Legacy - بدون Tokens) | ✅ متاح |
| `/api/v1/users/:id`                      | PATCH   | تحديث بيانات المستخدم             | ✅ متاح |
| `/api/v1/users/:id`                      | DELETE  | حذف مستخدم (Admin)                | ✅ متاح |
| `/api/v1/users/:id/photo`                | POST    | رفع صورة المستخدم                 | ✅ متاح |
| `/api/v1/users/:id/ban`                  | POST    | حظر مستخدم                        | ✅ متاح |
| `/api/v1/users/:id/unban`                | POST    | إلغاء حظر مستخدم                  | ✅ متاح |
| `/api/v1/users/:id/delete`               | POST    | حذف حساب المستخدم (Self)          | ✅ متاح |
| `/api/v1/users/:id/email/verify/send`    | POST    | إرسال كود تحقق الإيميل            | ✅ متاح |
| `/api/v1/users/:id/email/verify/confirm` | POST    | تأكيد كود تحقق الإيميل            | ✅ متاح |
| `/api/v1/users/:id/phone/verify/send`    | POST    | إرسال كود تحقق الهاتف             | ✅ متاح |
| `/api/v1/users/:id/phone/verify/confirm` | POST    | تأكيد كود تحقق الهاتف             | ✅ متاح |

#### 🔑 Auth Module (JWT + Sessions)

> هذه هي endpoints المعتمدة حالياً لتسجيل الدخول باستخدام JWT access token + refresh token (مع rotation عبر sessions في DB).

| المسار                                   | الطريقة | الوصف                                  | الحالة  |
| ---------------------------------------- | ------- | -------------------------------------- | ------- |
| `/api/v1/auth/user/login`                | POST    | تسجيل دخول مستخدم + إصدار tokens       | ✅ متاح |
| `/api/v1/auth/user/refresh`              | POST    | تجديد access token (refresh rotation)  | ✅ متاح |
| `/api/v1/auth/user/logout`               | POST    | تسجيل خروج (إبطال refresh token)       | ✅ متاح |
| `/api/v1/auth/me`                        | GET     | بيانات التوكن الحالي (Bearer required) | ✅ متاح |
| `/api/v1/auth/user/sessions`             | GET     | قائمة الجلسات (Bearer required)        | ✅ متاح |
| `/api/v1/auth/user/sessions/:id/revoke`  | POST    | إبطال جلسة (Bearer required)           | ✅ متاح |
| `/api/v1/auth/admin/login`               | POST    | تسجيل دخول Admin + إصدار tokens        | ✅ متاح |
| `/api/v1/auth/admin/refresh`             | POST    | تجديد tokens للأدمن                    | ✅ متاح |
| `/api/v1/auth/admin/logout`              | POST    | تسجيل خروج الأدمن                      | ✅ متاح |
| `/api/v1/auth/admin/sessions`            | GET     | جلسات الأدمن (Bearer required)         | ✅ متاح |
| `/api/v1/auth/admin/sessions/:id/revoke` | POST    | إبطال جلسة أدمن (Bearer required)      | ✅ متاح |

#### 💳 Users Payment Methods

| المسار                                            | الطريقة | الوصف                     | الحالة  |
| ------------------------------------------------- | ------- | ------------------------- | ------- |
| `/api/v1/users/:id/payment-methods`               | GET     | قائمة طرق الدفع           | ✅ متاح |
| `/api/v1/users/:id/payment-methods/setup-intent`  | POST    | إنشاء Stripe Setup Intent | ✅ متاح |
| `/api/v1/users/:id/payment-methods/attach`        | POST    | ربط طريقة دفع جديدة       | ✅ متاح |
| `/api/v1/users/:id/payment-methods/:pmId/default` | POST    | تعيين طريقة دفع افتراضية  | ✅ متاح |
| `/api/v1/users/:id/payment-methods/:pmId`         | DELETE  | حذف طريقة دفع             | ✅ متاح |

#### 📍 Users Addresses

| المسار                                           | الطريقة | الوصف                   | الحالة  |
| ------------------------------------------------ | ------- | ----------------------- | ------- |
| `/api/v1/users/addresses/meta`                   | GET     | بيانات العناوين الوصفية | ✅ متاح |
| `/api/v1/users/:id/addresses`                    | GET     | قائمة العناوين          | ✅ متاح |
| `/api/v1/users/:id/addresses`                    | POST    | إضافة عنوان جديد        | ✅ متاح |
| `/api/v1/users/:id/addresses/:addressId/default` | POST    | تعيين عنوان افتراضي     | ✅ متاح |
| `/api/v1/users/:id/addresses/:addressId`         | DELETE  | حذف عنوان               | ✅ متاح |

#### 🎫 Support Tickets Module

| المسار                              | الطريقة | الوصف             | الحالة  |
| ----------------------------------- | ------- | ----------------- | ------- |
| `/api/v1/support-tickets`           | GET     | قائمة التذاكر     | ✅ متاح |
| `/api/v1/support-tickets`           | POST    | إنشاء تذكرة جديدة | ✅ متاح |
| `/api/v1/support-tickets/:id/open`  | POST    | فتح تذكرة         | ✅ متاح |
| `/api/v1/support-tickets/:id/reply` | POST    | الرد على تذكرة    | ✅ متاح |

#### 💰 Payments Module

| المسار                                    | الطريقة | الوصف              | الحالة  |
| ----------------------------------------- | ------- | ------------------ | ------- |
| `/api/v1/payments/stripe/publishable-key` | GET     | مفتاح Stripe العام | ✅ متاح |

#### ⚙️ Site Settings Module

> ملاحظة: هذه endpoints تُستخدم لتخصيص صور الواجهة (Landing/About) من لوحة الأدمن.

| المسار                                              | الطريقة | الوصف                       | الحالة  |
| --------------------------------------------------- | ------- | --------------------------- | ------- |
| `/api/v1/site-settings/hero-image`                  | GET     | جلب صورة الهيرو (legacy)    | ✅ متاح |
| `/api/v1/site-settings/hero-image/upload`           | POST    | رفع صورة الهيرو (legacy)    | ✅ متاح |
| `/api/v1/site-settings/hero-image`                  | DELETE  | حذف صورة الهيرو (legacy)    | ✅ متاح |
| `/api/v1/site-settings/landing-images`              | GET     | جلب كل صور اللاندنج         | ✅ متاح |
| `/api/v1/site-settings/landing-images/:slot/upload` | POST    | رفع صورة لاندنج حسب الـslot | ✅ متاح |
| `/api/v1/site-settings/landing-images/:slot`        | DELETE  | حذف صورة لاندنج حسب الـslot | ✅ متاح |
| `/api/v1/site-settings/about-images`                | GET     | جلب كل صور صفحة About       | ✅ متاح |
| `/api/v1/site-settings/about-images/:slot/upload`   | POST    | رفع صورة About حسب الـslot  | ✅ متاح |
| `/api/v1/site-settings/about-images/:slot`          | DELETE  | حذف صورة About حسب الـslot  | ✅ متاح |

**Landing slots:**

- `hero`
- `whyFresh`
- `whyFast`
- `whySupport`
- `whyQuality`
- `carousel1` `carousel2` `carousel3` `carousel4`

**About slots:**

- `aboutHero`
- `aboutStory`
- `aboutFeature1`
- `aboutFeature2`
- `aboutFeature3`

**Upload notes:**

- Field name: `image` (multipart/form-data)
- Max size: 5MB
- Only `image/*` files allowed

---

### فحص صحة الخادم (Health Check)

```http
GET /api/v1/health
```

**Response (200):**

```json
{
  "status": "ok",
  "timestamp": "2026-02-01T12:00:00.000Z",
  "uptime": 3600.123
}
```

### صيغة البيانات

- **Request**: `application/json`
- **Response**: `application/json`

### المصادقة

> المشروع يدعم **JWT / Bearer Token** عبر Auth module (`/api/v1/auth/*`).
>
> - **Access token**: JWT قصير العمر (افتراضي 15m)
> - **Refresh token**: opaque token يتم تخزينه **hashed** في جدول sessions مع **rotation**
> - endpoint `POST /api/v1/users/login` ما زال موجوداً كـ **Legacy** لكنه لا يُصدر tokens (لا يُنصح به للواجهات الجديدة).

---

## 🔐 المصادقة

### تسجيل مستخدم جديد

```http
POST /api/v1/users/register
Content-Type: application/json
```

**Body:**

```json
{
  "fullName": "Ahmed Mohamed Ali",
  "countryCode": "LU",
  "countryName": "Luxembourg",
  "phone": "+352691234567",
  "email": "user@example.com",
  "password": "SecureP@ss123"
}
```

**Validation Notes:**

- `fullName` يجب أن يحتوي على **3 كلمات على الأقل**.
- `countryCode` يجب أن يكون ISO-3166 alpha-2 (مثل `LU`).

**Response (201):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "fullName": "Ahmed Mohamed Ali",
    "countryCode": "LU",
    "countryName": "Luxembourg",
    "phone": "+352691234567",
    "isBanned": false,
    "bannedAt": null,
    "phoneVerified": false,
    "email": "user@example.com",
    "emailVerified": false,
    "photoUrl": null,
    "createdAt": "2026-02-01T12:00:00.000Z"
  }
}
```

---

### تسجيل الدخول

```http
POST /api/v1/auth/user/login
Content-Type: application/json
```

**Body:**

```json
{
  "email": "user@example.com",
  "password": "SecureP@ss123"
}
```

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "fullName": "Ahmed Mohamed Ali",
    "countryCode": "LU",
    "countryName": "Luxembourg",
    "phone": "+352691234567",
    "isBanned": false,
    "bannedAt": null,
    "phoneVerified": false,
    "email": "user@example.com",
    "emailVerified": false,
    "photoUrl": null,
    "createdAt": "2026-02-01T12:00:00.000Z"
  },
  "tokens": {
    "accessToken": "jwt_access_token",
    "refreshToken": "opaque_refresh_token",
    "expiresIn": 900
  }
}
```

**Response (403) - Account banned:**

> إذا كان المستخدم محظوراً سيتم رفض تسجيل الدخول برسالة ثابتة يمكن للواجهة ترجمتها.

```json
{
  "statusCode": 403,
  "message": "ACCOUNT_BANNED",
  "error": "Forbidden"
}
```

---

### تجديد التوكن (Refresh)

```http
POST /api/v1/auth/user/refresh
Content-Type: application/json
```

**Body:**

```json
{
  "refreshToken": "opaque_refresh_token"
}
```

**Response (200):**

```json
{
  "ok": true,
  "tokens": {
    "accessToken": "jwt_access_token",
    "refreshToken": "opaque_refresh_token_rotated",
    "expiresIn": 900
  }
}
```

### تسجيل الخروج (Logout)

```http
POST /api/v1/auth/user/logout
Content-Type: application/json
```

**Body:**

```json
{
  "refreshToken": "opaque_refresh_token"
}
```

---

## 📍 نقاط النهاية

### الأدمن (Admin Staff / Employees)

> ملاحظة: هذه endpoints مخصصة للوحة الأدمن. حالياً لا يوجد توكن/صلاحيات على مستوى الـAPI (الحماية موجودة في واجهة الأدمن)، ويمكن إضافة حماية لاحقاً.

#### قائمة الموظفين

```http
GET /api/v1/admin-staff
```

**Response (200):**

```json
[
  {
    "id": "uuid",
    "name": "Ahmed Ali",
    "email": "admin123@moien.com",
    "photoUrl": "https://res.cloudinary.com/.../image/upload/...",
    "birthDate": "1995-02-03",
    "contractValidUntil": "2026-12-31",
    "jobTitle": "Dispatcher",
    "createdAt": "2026-02-03T10:00:00.000Z"
  }
]
```

#### إنشاء موظف (مع صورة)

```http
POST /api/v1/admin-staff
Content-Type: multipart/form-data
```

**Form fields:**

- `name` (string)
- `email` (string)
- `password` (string)
- `jobTitle` (string)
- `birthDate` (YYYY-MM-DD)
- `contractValidUntil` (YYYY-MM-DD)
- `photo` (file, optional)

**Response (201):**

```json
{
  "id": "uuid",
  "name": "Ahmed Ali",
  "email": "admin123@moien.com",
  "photoUrl": "https://res.cloudinary.com/.../image/upload/...",
  "birthDate": "1995-02-03",
  "contractValidUntil": "2026-12-31",
  "jobTitle": "Dispatcher",
  "createdAt": "2026-02-03T10:00:00.000Z"
}
```

#### تسجيل دخول الأدمن

```http
POST /api/v1/admin-staff/login
Content-Type: application/json
```

**Body:**

```json
{
  "email": "admin123@moien.com",
  "password": "yourPassword"
}
```

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "name": "Ahmed Ali",
    "email": "admin123@moien.com",
    "photoUrl": "https://res.cloudinary.com/.../image/upload/...",
    "birthDate": "1995-02-03",
    "contractValidUntil": "2026-12-31",
    "jobTitle": "Dispatcher",
    "createdAt": "2026-02-03T10:00:00.000Z"
  }
}
```

#### حذف موظف

```http
DELETE /api/v1/admin-staff/:id
```

**Response (200):**

```json
{ "ok": true }
```

### المستخدمين (Users)

#### قائمة المستخدمين

```http
GET /api/v1/users
```

**Response (200):**

```json
[
  {
    "id": "uuid",
    "fullName": "Ahmed Mohamed Ali",
    "countryCode": "LU",
    "countryName": "Luxembourg",
    "phone": "+352691234567",
    "isBanned": false,
    "bannedAt": null,
    "phoneVerified": false,
    "email": "user@example.com",
    "emailVerified": false,
    "photoUrl": null,
    "createdAt": "2026-02-01T12:00:00.000Z"
  }
]
```

#### تحديث الاسم

```http
PATCH /api/v1/users/:id
Content-Type: application/json
```

**Body:**

```json
{ "fullName": "Ahmed Mohamed Ali" }
```

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "fullName": "Ahmed Mohamed Ali",
    "countryCode": "LU",
    "countryName": "Luxembourg",
    "phone": "+352691234567",
    "isBanned": false,
    "bannedAt": null,
    "phoneVerified": false,
    "email": "user@example.com",
    "emailVerified": false,
    "photoUrl": null,
    "createdAt": "2026-02-01T12:00:00.000Z"
  }
}
```

#### رفع صورة المستخدم

```http
POST /api/v1/users/:id/photo
Content-Type: multipart/form-data
```

**Form fields:**

- `photo` (file, image/\*)

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "photoUrl": "https://res.cloudinary.com/.../image/upload/..."
  }
}
```

#### إرسال كود تحقق الإيميل

> يرسل كود مكوّن من **6 أحرف/أرقام** (Alphanumeric) إلى نفس الإيميل المُسجّل للمستخدم.
> الكود صالح لمدة **10 دقائق**.

```http
POST /api/v1/users/:id/email/verify/send
```

**Response (200):**

```json
{
  "ok": true,
  "email": "user@example.com",
  "expiresInSeconds": 600
}
```

#### تأكيد كود تحقق الإيميل

```http
POST /api/v1/users/:id/email/verify/confirm
Content-Type: application/json
```

**Body:**

```json
{ "code": "A1B2C3" }
```

#### إرسال كود تحقق الهاتف (WhatsApp)

> يرسل كود مكوّن من **6 أرقام** إلى رقم الهاتف المُسجّل للمستخدم عبر WhatsApp.
> الكود صالح لمدة **10 دقائق**.

```http
POST /api/v1/users/:id/phone/verify/send
```

**Response (200):**

```json
{
  "ok": true,
  "phone": "+352691234567",
  "expiresInSeconds": 600
}
```

#### تأكيد كود تحقق الهاتف

```http
POST /api/v1/users/:id/phone/verify/confirm
Content-Type: application/json
```

**Body:**

```json
{ "code": "123456" }
```

**Validation Notes:**

- `code` يجب أن يكون **6** أرقام بالضبط

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "phone": "+352691234567",
    "phoneVerified": true
  }
}
```

**Validation Notes:**

- `code` يجب أن يكون **6** أحرف بالضبط
- يسمح فقط بـ `A-Z` و `a-z` و `0-9`

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "emailVerified": true
  }
}
```

#### حذف حساب المستخدم (مع كلمة المرور)

> هذه endpoint مخصصة للمستخدم نفسه من واجهة المستخدم (Settings → Delete account).
> الهدف: تأكيد نية الحذف بإدخال كلمة المرور قبل حذف الحساب.

```http
POST /api/v1/users/:id/delete
Content-Type: application/json
```

**Body:**

```json
{ "password": "SecureP@ss123" }
```

**Response (200):**

```json
{ "ok": true }
```

**Response (401) - Wrong password:**

> يتم إرجاع رسالة ثابتة لتسهيل التعامل معها في الواجهة.

```json
{
  "statusCode": 401,
  "message": "WRONG_PASSWORD",
  "error": "Unauthorized"
}
```

#### حذف مستخدم (Admin)

```http
DELETE /api/v1/users/:id
```

**Response (200):**

```json
{ "ok": true }
```

#### حظر مستخدم (Ban)

> مخصص للاستخدام الإداري. يتطلب إدخال سبب الحظر (بالإنجليزية) ليتم تضمينه في رسالة البريد للمستخدم.

```http
POST /api/v1/users/:id/ban
Content-Type: application/json
```

**Body:**

```json
{ "reason": "Policy violation" }
```

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "isBanned": true,
    "bannedAt": "2026-02-03T10:00:00.000Z"
  }
}
```

#### إلغاء حظر مستخدم (Unban)

```http
POST /api/v1/users/:id/unban
```

**Response (200):**

```json
{
  "ok": true,
  "user": {
    "id": "uuid",
    "isBanned": false,
    "bannedAt": null
  }
}
```

---

### المطاعم (Restaurants)

#### قائمة المطاعم

```http
GET /restaurants
```

**Query Parameters:**
| المعامل | النوع | الوصف |
|---------|-------|-------|
| `page` | number | رقم الصفحة (افتراضي: 1) |
| `limit` | number | عدد النتائج (افتراضي: 20) |
| `lat` | number | خط العرض |
| `lng` | number | خط الطول |
| `radius` | number | نطاق البحث بالكيلومتر |
| `cuisine` | string | نوع المطبخ |
| `rating` | number | الحد الأدنى للتقييم |
| `sort` | string | ترتيب: distance, rating, delivery_time |

**مثال:**

```http
GET /restaurants?lat=49.6116&lng=6.1319&radius=5&cuisine=عربي&rating=4&sort=rating
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "restaurants": [
      {
        "id": "uuid",
        "name": "مطعم الشام",
        "description": "أشهى المأكولات الشامية الأصيلة",
        "image": "https://storage.moiendelivery.com/restaurants/uuid.jpg",
        "cuisines": ["عربي", "شامي"],
        "rating": 4.8,
        "reviewsCount": 256,
        "deliveryTime": "20-30",
        "deliveryFee": 2.5,
        "minOrder": 15.0,
        "distance": 1.2,
        "isOpen": true,
        "address": {
          "street": "45 شارع الحرية",
          "city": "لوكسمبورج"
        },
        "features": ["توصيل مجاني فوق 30€", "عروض"]
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 45,
      "totalPages": 3
    }
  }
}
```

---

#### تفاصيل مطعم

```http
GET /restaurants/:id
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "مطعم الشام",
    "description": "أشهى المأكولات الشامية الأصيلة",
    "images": ["https://storage.moiendelivery.com/restaurants/uuid-1.jpg", "https://storage.moiendelivery.com/restaurants/uuid-2.jpg"],
    "cuisines": ["عربي", "شامي"],
    "rating": 4.8,
    "reviewsCount": 256,
    "deliveryTime": "20-30",
    "deliveryFee": 2.5,
    "minOrder": 15.0,
    "address": {
      "street": "45 شارع الحرية",
      "city": "لوكسمبورج",
      "coordinates": {
        "lat": 49.6116,
        "lng": 6.1319
      }
    },
    "workingHours": {
      "monday": { "open": "10:00", "close": "22:00" },
      "tuesday": { "open": "10:00", "close": "22:00" },
      "wednesday": { "open": "10:00", "close": "22:00" },
      "thursday": { "open": "10:00", "close": "22:00" },
      "friday": { "open": "10:00", "close": "23:00" },
      "saturday": { "open": "11:00", "close": "23:00" },
      "sunday": { "open": "11:00", "close": "21:00" }
    },
    "menu": {
      "categories": [
        {
          "id": "uuid",
          "name": "المقبلات",
          "items": [
            {
              "id": "uuid",
              "name": "حمص",
              "description": "حمص طازج مع زيت الزيتون",
              "price": 5.5,
              "image": "https://storage.moiendelivery.com/items/uuid.jpg",
              "isAvailable": true,
              "options": [
                {
                  "name": "الإضافات",
                  "type": "multiple",
                  "choices": [
                    { "name": "لحم", "price": 2.0 },
                    { "name": "صنوبر", "price": 1.5 }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  }
}
```

---

### الطلبات (Orders)

#### إنشاء طلب

```http
POST /orders
Authorization: Bearer <access_token>
```

**Body:**

```json
{
  "restaurantId": "uuid",
  "items": [
    {
      "menuItemId": "uuid",
      "quantity": 2,
      "notes": "بدون بصل",
      "options": [
        {
          "name": "الحجم",
          "value": "كبير",
          "price": 3.0
        }
      ]
    }
  ],
  "deliveryAddress": {
    "id": "uuid"
  },
  "paymentMethod": "card",
  "promoCode": "FIRST50",
  "tip": 5.0,
  "notes": "الرجاء الاتصال عند الوصول"
}
```

**Response (201):**

```json
{
  "success": true,
  "data": {
    "order": {
      "id": "uuid",
      "orderNumber": "MD-2026-0001",
      "status": "pending",
      "restaurant": {
        "id": "uuid",
        "name": "مطعم الشام"
      },
      "items": [...],
      "subtotal": 25.00,
      "deliveryFee": 2.50,
      "discount": 12.50,
      "tip": 5.00,
      "total": 20.00,
      "estimatedDelivery": "30-40 min",
      "createdAt": "2026-02-01T12:00:00.000Z"
    },
    "payment": {
      "clientSecret": "pi_xxx_secret_xxx"
    }
  }
}
```

---

#### تتبع الطلب

```http
GET /orders/:id/track
Authorization: Bearer <access_token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "orderId": "uuid",
    "status": "on_the_way",
    "timeline": [
      {
        "status": "pending",
        "timestamp": "2026-02-01T12:00:00.000Z"
      },
      {
        "status": "accepted",
        "timestamp": "2026-02-01T12:02:00.000Z"
      },
      {
        "status": "preparing",
        "timestamp": "2026-02-01T12:05:00.000Z"
      },
      {
        "status": "ready",
        "timestamp": "2026-02-01T12:20:00.000Z"
      },
      {
        "status": "picked",
        "timestamp": "2026-02-01T12:25:00.000Z"
      },
      {
        "status": "on_the_way",
        "timestamp": "2026-02-01T12:26:00.000Z"
      }
    ],
    "driver": {
      "id": "uuid",
      "name": "محمد",
      "phone": "+352691234567",
      "avatar": "https://storage.moiendelivery.com/drivers/uuid.jpg",
      "rating": 4.9,
      "vehicle": {
        "type": "motorcycle",
        "color": "أسود",
        "plate": "ABC 123"
      },
      "location": {
        "lat": 49.61,
        "lng": 6.13
      }
    },
    "eta": 8,
    "distance": 1.2
  }
}
```

---

### السائقين (Drivers)

#### تحديث الموقع

```http
PATCH /drivers/location
Authorization: Bearer <access_token>
```

**Body:**

```json
{
  "lat": 49.6116,
  "lng": 6.1319,
  "heading": 90,
  "speed": 25
}
```

---

#### الطلبات المتاحة

```http
GET /drivers/deliveries/available
Authorization: Bearer <access_token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "deliveries": [
      {
        "id": "uuid",
        "orderNumber": "MD-2026-0001",
        "restaurant": {
          "name": "مطعم الشام",
          "address": "45 شارع الحرية",
          "coordinates": { "lat": 49.6116, "lng": 6.1319 }
        },
        "customer": {
          "address": "123 شارع الاستقلال",
          "coordinates": { "lat": 49.62, "lng": 6.14 }
        },
        "distance": {
          "toRestaurant": 0.5,
          "toCustomer": 1.8,
          "total": 2.3
        },
        "earnings": 8.5,
        "tip": 5.0,
        "itemsCount": 3
      }
    ]
  }
}
```

---

## 📊 رموز الاستجابة

| الرمز | الوصف                  |
| ----- | ---------------------- |
| `200` | نجاح                   |
| `201` | تم الإنشاء             |
| `204` | لا محتوى               |
| `400` | طلب خاطئ               |
| `401` | غير مصرح               |
| `403` | ممنوع                  |
| `404` | غير موجود              |
| `409` | تعارض                  |
| `422` | كيان غير قابل للمعالجة |
| `429` | طلبات كثيرة جداً       |
| `500` | خطأ في الخادم          |

---

## ❌ معالجة الأخطاء

### صيغة الخطأ

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "خطأ في التحقق من البيانات",
    "details": [
      {
        "field": "email",
        "message": "البريد الإلكتروني غير صالح"
      }
    ]
  }
}
```

### رموز الأخطاء

| الرمز                  | الوصف           |
| ---------------------- | --------------- |
| `VALIDATION_ERROR`     | خطأ في التحقق   |
| `AUTHENTICATION_ERROR` | خطأ في المصادقة |
| `AUTHORIZATION_ERROR`  | غير مصرح        |
| `NOT_FOUND`            | غير موجود       |
| `CONFLICT`             | تعارض           |
| `RATE_LIMIT_EXCEEDED`  | تجاوز الحد      |
| `INTERNAL_ERROR`       | خطأ داخلي       |

---

## ⏱️ التحديد (Rate Limiting)

| النوع    | الحد          |
| -------- | ------------- |
| عام      | 100 طلب/دقيقة |
| المصادقة | 10 طلب/دقيقة  |
| الدفع    | 20 طلب/دقيقة  |

### Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1706788800
```

---

## 📄 الصفحات (Pagination)

### Query Parameters

| المعامل | النوع  | الافتراضي |
| ------- | ------ | --------- |
| `page`  | number | 1         |
| `limit` | number | 20        |

### Response

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5,
    "hasNext": true,
    "hasPrev": false
  }
}
```

---

## 🔗 Swagger/OpenAPI

التوثيق التفاعلي متاح على:

- **التطوير**: `http://localhost:3000/api/docs`
- **الإنتاج**: `https://api.moiendelivery.com/docs`

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [🗄️ قاعدة البيانات](DATABASE.md)

</div>
