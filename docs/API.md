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

### صيغة البيانات

- **Request**: `application/json`
- **Response**: `application/json`

### المصادقة

جميع الطلبات (ماعدا التسجيل والدخول) تتطلب Bearer Token:

```http
Authorization: Bearer <access_token>
```

---

## 🔐 المصادقة

### تسجيل مستخدم جديد

```http
POST /auth/register
```

**Body:**

```json
{
  "email": "user@example.com",
  "password": "SecureP@ss123",
  "name": "أحمد محمد",
  "phone": "+352691234567",
  "role": "customer"
}
```

**Response (201):**

```json
{
  "success": true,
  "message": "تم التسجيل بنجاح. يرجى تأكيد بريدك الإلكتروني.",
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "name": "أحمد محمد",
      "phone": "+352691234567",
      "role": "customer",
      "isVerified": false,
      "createdAt": "2026-02-01T12:00:00.000Z"
    }
  }
}
```

---

### تسجيل الدخول

```http
POST /auth/login
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
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
    "expiresIn": 900,
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "name": "أحمد محمد",
      "role": "customer"
    }
  }
}
```

---

### تجديد التوكن

```http
POST /auth/refresh
```

**Body:**

```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

---

### تسجيل الخروج

```http
POST /auth/logout
Authorization: Bearer <access_token>
```

---

## 📍 نقاط النهاية

### المستخدمين (Users)

#### الحصول على الملف الشخصي

```http
GET /users/me
Authorization: Bearer <access_token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "أحمد محمد",
    "phone": "+352691234567",
    "avatar": "https://storage.moiendelivery.com/avatars/uuid.jpg",
    "role": "customer",
    "isVerified": true,
    "addresses": [
      {
        "id": "uuid",
        "label": "المنزل",
        "address": "123 شارع الاستقلال",
        "city": "لوكسمبورج",
        "coordinates": {
          "lat": 49.6116,
          "lng": 6.1319
        },
        "isDefault": true
      }
    ],
    "createdAt": "2026-02-01T12:00:00.000Z"
  }
}
```

#### تحديث الملف الشخصي

```http
PATCH /users/me
Authorization: Bearer <access_token>
```

**Body:**

```json
{
  "name": "أحمد محمد علي",
  "phone": "+352691234568"
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
