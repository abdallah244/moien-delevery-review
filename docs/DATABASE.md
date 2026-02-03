# 🗄️ توثيق قاعدة البيانات | Database Documentation

<div align="center">

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue.svg)]()
[![TypeORM](https://img.shields.io/badge/TypeORM-Latest-orange.svg)]()

**مخطط قاعدة بيانات Moien Delivery**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [مخطط العلاقات (ERD)](#-مخطط-العلاقات-erd)
- [الجداول](#-الجداول)
- [الفهارس](#-الفهارس)
- [الهجرات](#-الهجرات)

---

## 🎯 نظرة عامة

### التقنيات

| التقنية       | الاستخدام               |
| ------------- | ----------------------- |
| PostgreSQL 15 | قاعدة البيانات الرئيسية |
| TypeORM       | ORM                     |
| PostGIS       | البيانات الجغرافية      |
| Redis         | التخزين المؤقت          |

### الإعدادات

الإعدادات موجودة في `backend/src/config/database.config.ts`:

```typescript
// database.config.ts
import { registerAs } from "@nestjs/config";

export default registerAs("database", () => ({
  type: "postgres",
  host: process.env.DATABASE_HOST || "localhost",
  port: parseInt(process.env.DATABASE_PORT ?? "5432", 10),
  username: process.env.DATABASE_USER || "postgres",
  password: process.env.DATABASE_PASSWORD || "password",
  database: process.env.DATABASE_NAME || "moien_delivery",
  entities: ["dist/**/*.entity.js"],
  migrations: ["dist/database/migrations/*.js"],
  synchronize: process.env.NODE_ENV === "development",
  logging: process.env.NODE_ENV === "development",
}));
```

### متغيرات البيئة المطلوبة

```env
# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=moien_delivery
DATABASE_USER=postgres
DATABASE_PASSWORD=your_secure_password
```

### إنشاء قاعدة البيانات

```sql
-- PostgreSQL
CREATE DATABASE moien_delivery;
```

---

## 📊 مخطط العلاقات (ERD)

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                 DATABASE SCHEMA                                  │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│  ┌──────────────────┐                      ┌──────────────────┐                 │
│  │      users       │                      │   restaurants    │                 │
│  ├──────────────────┤                      ├──────────────────┤                 │
│  │ PK id            │                      │ PK id            │                 │
│  │    email         │──────────────────────│ FK owner_id      │                 │
│  │    password_hash │                      │    name          │                 │
│  │    name          │                      │    description   │                 │
│  │    phone         │                      │    logo          │                 │
│  │    avatar        │                      │    cover_image   │                 │
│  │    role          │                      │    address       │                 │
│  │    is_verified   │                      │    location      │                 │
│  │    created_at    │                      │    phone         │                 │
│  │    updated_at    │                      │    rating        │                 │
│  └────────┬─────────┘                      │    is_active     │                 │
│           │                                │    created_at    │                 │
│           │                                └────────┬─────────┘                 │
│           │                                         │                           │
│           │    ┌──────────────────┐                 │                           │
│           │    │   user_addresses │                 │    ┌──────────────────┐   │
│           │    ├──────────────────┤                 │    │  menu_categories │   │
│           └────│ FK user_id       │                 │    ├──────────────────┤   │
│                │ PK id            │                 └────│ FK restaurant_id │   │
│                │    label         │                      │ PK id            │   │
│                │    address       │                      │    name          │   │
│                │    building      │                      │    sort_order    │   │
│                │    floor         │                      └────────┬─────────┘   │
│                │    apartment     │                               │             │
│                │    location      │                               │             │
│                │    is_default    │                               │             │
│                └──────────────────┘        ┌──────────────────┐   │             │
│                                            │    menu_items    │   │             │
│                                            ├──────────────────┤   │             │
│                                            │ PK id            │───┘             │
│  ┌──────────────────┐                      │ FK category_id   │                 │
│  │     drivers      │                      │    name          │                 │
│  ├──────────────────┤                      │    description   │                 │
│  │ PK id            │                      │    price         │                 │
│  │ FK user_id       │                      │    image         │                 │
│  │    license_no    │                      │    is_available  │                 │
│  │    vehicle_type  │                      │    options       │                 │
│  │    vehicle_plate │                      └──────────────────┘                 │
│  │    vehicle_color │                                                           │
│  │    status        │                                                           │
│  │    current_loc   │                      ┌──────────────────┐                 │
│  │    is_available  │                      │      orders      │                 │
│  │    rating        │                      ├──────────────────┤                 │
│  │    total_trips   │◄─────────────────────│ FK driver_id     │                 │
│  └──────────────────┘                      │ FK user_id       │                 │
│                                            │ FK restaurant_id │                 │
│                                            │ PK id            │                 │
│                                            │    order_number  │                 │
│                                            │    status        │                 │
│                                            │    subtotal      │                 │
│                                            │    delivery_fee  │                 │
│                                            │    discount      │                 │
│                                            │    tip           │                 │
│                                            │    total         │                 │
│                                            │    address       │                 │
│                                            │    notes         │                 │
│                                            │    created_at    │                 │
│                                            └────────┬─────────┘                 │
│                                                     │                           │
│           ┌─────────────────────────────────────────┼────────────────┐          │
│           │                                         │                │          │
│           ▼                                         ▼                ▼          │
│  ┌──────────────────┐                ┌──────────────────┐  ┌──────────────────┐ │
│  │   order_items    │                │     payments     │  │     reviews      │ │
│  ├──────────────────┤                ├──────────────────┤  ├──────────────────┤ │
│  │ PK id            │                │ PK id            │  │ PK id            │ │
│  │ FK order_id      │                │ FK order_id      │  │ FK user_id       │ │
│  │ FK menu_item_id  │                │    amount        │  │ FK order_id      │ │
│  │    quantity      │                │    method        │  │    rating        │ │
│  │    unit_price    │                │    status        │  │    comment       │ │
│  │    options       │                │    transaction_id│  │    type          │ │
│  │    notes         │                │    created_at    │  │    target_id     │ │
│  └──────────────────┘                └──────────────────┘  │    created_at    │ │
│                                                            └──────────────────┘ │
│                                                                                  │
│  ┌──────────────────┐                ┌──────────────────┐  ┌──────────────────┐ │
│  │   promotions     │                │  notifications   │  │ delivery_zones   │ │
│  ├──────────────────┤                ├──────────────────┤  ├──────────────────┤ │
│  │ PK id            │                │ PK id            │  │ PK id            │ │
│  │    code          │                │ FK user_id       │  │    name          │ │
│  │    type          │                │    title         │  │    polygon       │ │
│  │    value         │                │    body          │  │    delivery_fee  │ │
│  │    min_order     │                │    data          │  │    min_order     │ │
│  │    max_discount  │                │    is_read       │  │    is_active     │ │
│  │    start_date    │                │    created_at    │  └──────────────────┘ │
│  │    end_date      │                └──────────────────┘                       │
│  │    usage_limit   │                                                           │
│  │    used_count    │                                                           │
│  └──────────────────┘                                                           │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📝 الجداول

### users (المستخدمين)

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    fullName VARCHAR(180) NOT NULL,
    countryCode VARCHAR(2) NOT NULL,
    countryName VARCHAR(120) NOT NULL,
    phone VARCHAR(30) NOT NULL,

    -- Ban / disable
    isBanned BOOLEAN DEFAULT FALSE,
    banReason VARCHAR(500),
    bannedAt TIMESTAMPTZ,

    -- Phone verification (WhatsApp)
    phoneVerified BOOLEAN DEFAULT FALSE,
    phoneVerificationCodeHash VARCHAR(200),
    phoneVerificationExpiresAt TIMESTAMPTZ,
    email VARCHAR(254) UNIQUE NOT NULL,

    -- Email verification
    emailVerified BOOLEAN DEFAULT FALSE,
    emailVerificationCodeHash VARCHAR(200),
    emailVerificationExpiresAt TIMESTAMPTZ,

    -- Profile photo
    photoUrl VARCHAR(500),

    passwordHash VARCHAR(200) NOT NULL,
    createdAt TIMESTAMPTZ DEFAULT NOW(),
    updatedAt TIMESTAMPTZ DEFAULT NOW()
);
```

---

### restaurants (المطاعم)

```sql
CREATE TABLE restaurants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    owner_id UUID REFERENCES users(id) ON DELETE SET NULL,
    name VARCHAR(200) NOT NULL,
    name_ar VARCHAR(200),
    description TEXT,
    description_ar TEXT,
    logo VARCHAR(500),
    cover_image VARCHAR(500),
    cuisines TEXT[] DEFAULT '{}',
    address VARCHAR(500) NOT NULL,
    location GEOGRAPHY(POINT, 4326) NOT NULL,
    phone VARCHAR(20),
    email VARCHAR(255),
    rating DECIMAL(2,1) DEFAULT 0,
    reviews_count INTEGER DEFAULT 0,
    delivery_time_min INTEGER DEFAULT 20,
    delivery_time_max INTEGER DEFAULT 40,
    delivery_fee DECIMAL(10,2) DEFAULT 0,
    min_order DECIMAL(10,2) DEFAULT 0,
    is_active BOOLEAN DEFAULT FALSE,
    is_verified BOOLEAN DEFAULT FALSE,
    working_hours JSONB DEFAULT '{}',
    features TEXT[] DEFAULT '{}',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- فهرس جغرافي
CREATE INDEX idx_restaurants_location ON restaurants USING GIST(location);
```

---

### menu_categories (أقسام القائمة)

```sql
CREATE TABLE menu_categories (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    restaurant_id UUID REFERENCES restaurants(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    name_ar VARCHAR(100),
    description TEXT,
    image VARCHAR(500),
    sort_order INTEGER DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### menu_items (أصناف القائمة)

```sql
CREATE TABLE menu_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    category_id UUID REFERENCES menu_categories(id) ON DELETE CASCADE,
    name VARCHAR(200) NOT NULL,
    name_ar VARCHAR(200),
    description TEXT,
    description_ar TEXT,
    price DECIMAL(10,2) NOT NULL,
    image VARCHAR(500),
    is_available BOOLEAN DEFAULT TRUE,
    is_popular BOOLEAN DEFAULT FALSE,
    preparation_time INTEGER DEFAULT 15,
    calories INTEGER,
    options JSONB DEFAULT '[]',
    allergens TEXT[] DEFAULT '{}',
    sort_order INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- مثال على options
-- [
--   {
--     "name": "الحجم",
--     "type": "single",
--     "required": true,
--     "choices": [
--       {"name": "صغير", "price": 0},
--       {"name": "وسط", "price": 2},
--       {"name": "كبير", "price": 4}
--     ]
--   }
-- ]
```

---

### orders (الطلبات)

```sql
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_number VARCHAR(20) UNIQUE NOT NULL,
    user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    restaurant_id UUID REFERENCES restaurants(id) ON DELETE SET NULL,
    driver_id UUID REFERENCES drivers(id) ON DELETE SET NULL,
    status VARCHAR(30) NOT NULL DEFAULT 'pending',
    subtotal DECIMAL(10,2) NOT NULL,
    delivery_fee DECIMAL(10,2) DEFAULT 0,
    discount DECIMAL(10,2) DEFAULT 0,
    tip DECIMAL(10,2) DEFAULT 0,
    total DECIMAL(10,2) NOT NULL,
    promo_code VARCHAR(50),
    delivery_address JSONB NOT NULL,
    notes TEXT,
    scheduled_at TIMESTAMP,
    accepted_at TIMESTAMP,
    preparing_at TIMESTAMP,
    ready_at TIMESTAMP,
    picked_at TIMESTAMP,
    delivered_at TIMESTAMP,
    cancelled_at TIMESTAMP,
    cancellation_reason TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- الحالات
-- status: 'pending' | 'accepted' | 'rejected' | 'preparing' |
--         'ready' | 'picked' | 'on_the_way' | 'delivered' | 'cancelled'
```

---

### order_items (عناصر الطلب)

```sql
CREATE TABLE order_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID REFERENCES orders(id) ON DELETE CASCADE,
    menu_item_id UUID REFERENCES menu_items(id) ON DELETE SET NULL,
    name VARCHAR(200) NOT NULL,
    quantity INTEGER NOT NULL DEFAULT 1,
    unit_price DECIMAL(10,2) NOT NULL,
    options JSONB DEFAULT '[]',
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### drivers (السائقين)

```sql
CREATE TABLE drivers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID UNIQUE REFERENCES users(id) ON DELETE CASCADE,
    license_number VARCHAR(50) NOT NULL,
    license_expiry DATE NOT NULL,
    id_document VARCHAR(500),
    vehicle_type VARCHAR(30) NOT NULL,
    vehicle_plate VARCHAR(20) NOT NULL,
    vehicle_color VARCHAR(30),
    vehicle_model VARCHAR(100),
    status VARCHAR(20) DEFAULT 'pending',
    current_location GEOGRAPHY(POINT, 4326),
    is_available BOOLEAN DEFAULT FALSE,
    is_online BOOLEAN DEFAULT FALSE,
    rating DECIMAL(2,1) DEFAULT 5.0,
    total_deliveries INTEGER DEFAULT 0,
    total_earnings DECIMAL(12,2) DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- vehicle_type: 'bicycle' | 'motorcycle' | 'car'
-- status: 'pending' | 'approved' | 'rejected' | 'suspended'

CREATE INDEX idx_drivers_location ON drivers USING GIST(current_location);
```

---

### payments (المدفوعات)

```sql
CREATE TABLE payments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID REFERENCES orders(id) ON DELETE SET NULL,
    amount DECIMAL(10,2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'EUR',
    method VARCHAR(30) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    provider VARCHAR(30),
    transaction_id VARCHAR(255),
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- method: 'cash' | 'card' | 'wallet' | 'apple_pay' | 'google_pay'
-- status: 'pending' | 'processing' | 'completed' | 'failed' | 'refunded'
```

---

### reviews (التقييمات)

```sql
CREATE TABLE reviews (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    order_id UUID REFERENCES orders(id) ON DELETE SET NULL,
    target_type VARCHAR(20) NOT NULL,
    target_id UUID NOT NULL,
    rating INTEGER NOT NULL CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    images TEXT[] DEFAULT '{}',
    response TEXT,
    response_at TIMESTAMP,
    is_visible BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- target_type: 'restaurant' | 'driver'
```

---

### promotions (العروض)

```sql
CREATE TABLE promotions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    code VARCHAR(50) UNIQUE NOT NULL,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    type VARCHAR(20) NOT NULL,
    value DECIMAL(10,2) NOT NULL,
    min_order DECIMAL(10,2) DEFAULT 0,
    max_discount DECIMAL(10,2),
    usage_limit INTEGER,
    used_count INTEGER DEFAULT 0,
    per_user_limit INTEGER DEFAULT 1,
    start_date TIMESTAMP NOT NULL,
    end_date TIMESTAMP NOT NULL,
    restaurant_id UUID REFERENCES restaurants(id),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- type: 'percentage' | 'fixed' | 'free_delivery'
```

---

### notifications (الإشعارات)

```sql
CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    type VARCHAR(50) NOT NULL,
    title VARCHAR(200) NOT NULL,
    body TEXT NOT NULL,
    data JSONB DEFAULT '{}',
    is_read BOOLEAN DEFAULT FALSE,
    read_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_notifications_user ON notifications(user_id, is_read);
```

---

## 🔍 الفهارس

```sql
-- Users
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_phone ON users(phone);
CREATE INDEX idx_users_role ON users(role);

-- Restaurants
CREATE INDEX idx_restaurants_owner ON restaurants(owner_id);
CREATE INDEX idx_restaurants_active ON restaurants(is_active);
CREATE INDEX idx_restaurants_rating ON restaurants(rating DESC);

-- Menu Items
CREATE INDEX idx_menu_items_category ON menu_items(category_id);
CREATE INDEX idx_menu_items_available ON menu_items(is_available);

-- Orders
CREATE INDEX idx_orders_user ON orders(user_id);
CREATE INDEX idx_orders_restaurant ON orders(restaurant_id);
CREATE INDEX idx_orders_driver ON orders(driver_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created ON orders(created_at DESC);

-- Reviews
CREATE INDEX idx_reviews_target ON reviews(target_type, target_id);
CREATE INDEX idx_reviews_user ON reviews(user_id);
```

---

## 🔄 الهجرات

### إنشاء هجرة جديدة

```bash
npm run migration:generate -- -n CreateUsersTable
```

### تشغيل الهجرات

```bash
npm run migration:run
```

### التراجع عن آخر هجرة

```bash
npm run migration:revert
```

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [📡 API](API.md)

</div>
