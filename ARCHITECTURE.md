# Moien Delivery — Architecture

> Last updated: 12 March 2026 · v0.0.17+

---

## Table of Contents

1. [Overview](#overview)
2. [System Diagram](#system-diagram)
3. [Monorepo Structure](#monorepo-structure)
4. [Backend (NestJS 11)](#backend-nestjs-11)
5. [Web Frontend (Angular 21)](#web-frontend-angular-21)
6. [Database (PostgreSQL 16)](#database-postgresql-16)
7. [Real-Time (Socket.IO 4.8)](#real-time-socketio-48)
8. [Security & Auth](#security--auth)
9. [External Integrations](#external-integrations)
10. [Deployment Architecture](#deployment-architecture)

---

## Overview

Moien Delivery is a food-delivery platform serving Luxembourg. Four user roles: **customers**, **restaurant partners**, **delivery drivers**, and **admins**.

| Component | Technology | Status |
|-----------|-----------|--------|
| Backend API | NestJS 11, TypeORM 0.3.28, PostgreSQL 16 | ✅ Production |
| Web Frontend | Angular 21, Standalone Components, Signals | ✅ Production |
| Mobile App | Flutter (planned) | ❌ Not started |
| Infrastructure | Hostinger VPS, Nginx, PM2, UFW | ✅ Production |

---

## System Diagram

```
┌──────────────────────────────────────────────────────────┐
│                        CLIENTS                           │
│  ┌──────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │ Customer │  │  Restaurant   │  │  Driver          │  │
│  │   Web    │  │  Dashboard    │  │  Dashboard (web) │  │
│  └────┬─────┘  └──────┬────────┘  └───────┬──────────┘  │
└───────┼────────────────┼──────────────────┼──────────────┘
        │  HTTPS / WSS   │                  │
        ▼                ▼                  ▼
┌──────────────────────────────────────────────────────────┐
│                    NGINX (Reverse Proxy)                  │
│   moiendelivery.lu → Angular SPA (static files)          │
│   api.moiendelivery.lu → NestJS :3000                    │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│              NESTJS BACKEND (Port 3000)                   │
│  ┌──────────┐  ┌────────────┐  ┌──────────────────────┐  │
│  │ REST API │  │ WebSocket  │  │   Background Jobs    │  │
│  │ 13 mods  │  │ 8 gateways │  │   (Bull + Redis)     │  │
│  └────┬─────┘  └─────┬──────┘  └──────────┬───────────┘  │
│       └───────────────┴────────────────────┘              │
│                       │                                   │
│  ┌────────────────────┴────────────────────────────────┐  │
│  │            CommonModule (26 services)               │  │
│  │  3 controllers · 2 middleware · 2 interceptors · 1  │  │
│  │  filter · Security · Cache · Logger · SMS · Email   │  │
│  └─────────────────────────────────────────────────────┘  │
└───────────────────────┬──────────────────────────────────┘
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   ┌─────────┐   ┌───────────┐   ┌───────────┐
   │ Postgres │   │   Redis   │   │ External  │
   │  16 ent. │   │  (cache/  │   │ Stripe    │
   │  25 migr │   │   queue)  │   │ PayPal    │
   └─────────┘   └───────────┘   │ Twilio    │
                                  │ Cloudinary│
                                  └───────────┘
```

---

## Monorepo Structure

```
moien-delivery/
├── package.json              # npm workspaces root ["backend", "wfrontend"]
├── backend/                  # NestJS 11 API
│   ├── src/
│   │   ├── main.ts           # Bootstrap: rawBody, CORS, Helmet, ValidationPipe
│   │   ├── app.module.ts     # 13 feature modules + CommonModule + ThrottlerGuard
│   │   ├── app.controller.ts # Health, access gate, docs
│   │   ├── config/           # 7 typed configs
│   │   ├── common/           # 26 services, 3 controllers, 2 middleware, 2 interceptors, 1 filter
│   │   ├── modules/          # 13 feature modules (14 dirs, drivers has sub-modules)
│   │   └── database/         # DataSource + seed + 25 migrations
│   └── test/                 # E2E (Jest 30)
│
├── wfrontend/                # Angular 21 web app
│   ├── src/app/
│   │   ├── pages/            # 7 page groups
│   │   ├── services/         # 38 services
│   │   ├── guards/           # 11 guards
│   │   ├── interceptors/     # 4 interceptors
│   │   ├── components/       # 12 shared components
│   │   ├── layouts/          # 2 layouts (user + admin)
│   │   └── utils/            # Utilities
│   └── src/environments/     # dev / prod configs
│
├── docs/                     # Technical documentation
├── markdown/                 # Project-level docs
├── ops/                      # Nginx + PM2 configs
└── uploads/                  # Server-side uploads
```

---

## Backend (NestJS 11)

### Bootstrap (main.ts)

- `rawBody: true` for Stripe webhook signature verification
- Global pipes: `ValidationPipe` (whitelist, forbidNonWhitelisted, transform)
- Global filters: `AllExceptionsFilter`
- Global interceptors: `HttpLoggingInterceptor`, optional `ResponseTransformInterceptor` (via `RESPONSE_ENVELOPE` env)
- Middleware: `ServerAccessService` (access gate), `RequestIdMiddleware`, `CsrfMiddleware`
- CORS whitelist: `localhost:4200`, `localhost:5173`, `moiendelivery.lu`, `www.moiendelivery.lu`, `api.moiendelivery.lu`
- Static: `/uploads` served from disk
- API prefix: `API_PREFIX` env (default `api/v1`)

### 13 Feature Modules (in AppModule)

| Module | Entities | Controllers | Gateways | Key Features |
|--------|---------|-------------|----------|-------------|
| **AdminStaff** | `AdminStaffEntity` | `AdminStaffController`, `AdminDashboardController`, `AdminAnalystController`, `AdminProfileController`, `AdminProviderAnalyticsController`, `AdminProviderNotificationsController` | `AdminAnalystGateway` | Staff CRUD, dashboard overview (counts + pending), analytics, provider management |
| **Auth** | `AuthSessionEntity` | `AuthController` | — | User/Admin/Driver/Partner JWT, refresh rotation, Google OAuth, sessions, forgot-password (3-step: request→verify→reset) |
| **Users** | `UserEntity`, `UserAddressEntity`, `UserPaymentMethodEntity` | `UsersController` | `UsersGateway` | CRUD, addresses, Stripe payment methods, email/phone OTP (SMS/WhatsApp), photo upload, **Moien Coins wallet**, ban/unban |
| **Restaurants** | `RestaurantEntity`, `RestaurantCategoryEntity`, `MenuItemEntity` | `RestaurantsController`, `MenuItemsController`, `RestaurantCategoriesController` | — | Full CRUD, search/filter, categories, menu items with **addons (JSONB)**, geocode proxy |
| **Orders** | `OrderEntity`, `OrderItemEntity` | `OrdersController` | `PartnerOrdersGateway`, `UserOrdersGateway` | Multi-restaurant cart, checkout (cash/Stripe/PayPal/**Moien Coins**), tracking, driver dispatch, **cashback 1%**, **short order IDs**, email+SMS confirmation, **auto-apply promotional offers**, **new user free delivery** |
| **Drivers** | `DriverEntity`, `DriverApplicationEntity` | `DriversController`, `AdminDriversController`, `DriverApplicationsController` | `DriverGateway`, `AdminDriversGateway` | Registration, applications, online/location, nearest-driver dispatch, order lifecycle, admin messaging (Redis inbox) |
| **Payments** | — | `PaymentsController` | — | Stripe publishable key, Stripe webhook (raw body), PayPal client-id |
| **Promotions** | `PromotionEntity`, `PromotionRestaurantEntity`, `PromotionRedemptionEntity`, `PromotionalOfferEntity`, `OfferRestaurantEntity` | `PromotionsController`, `PromotionalOffersController` | — | Promo codes (validate/redeem), promotional offer banners (auto-apply at checkout) |
| **Ratings** | `PlatformRatingEntity`, `RestaurantRatingEntity` | `RatingsController`, `PartnerRatingsController` | — | Restaurant + platform reviews |
| **Notifications** | `NotificationEntity` | `NotificationsController` | `NotificationsGateway` | User notifications, mark read, delete, TTL cleanup (10 days) |
| **PartnerPlaces** | `PartnerAccountEntity`, `PartnerPlaceBranchEntity`, `PartnerPlaceRequestEntity` | `PartnerPlacesController` | `PartnerPlacesGateway`, `PartnerNotificationsGateway` | Partner onboarding workflow, menu/category CRUD, branches, order management, **accept/reject orders**, **sales analytics**, image upload, test orders (dev) |
| **SiteSettings** | `SiteSettingsEntity` | `SiteSettingsController` | `SiteSettingsGateway` | Landing hero/images, about images, **coins images**, real-time updates |
| **SupportTickets** | `SupportTicketEntity` | `SupportTicketsController` | `SupportTicketsGateway` | Create (public), admin list/reply/delete |

### CommonModule — 26 Shared Services

| Category | Services |
|----------|---------|
| **Security** (6) | `EncryptionService`, `IpGuardService`, `JwtAuthService`, `RateLimiterService`, `SanitizerService`, `ServerAccessService` |
| **Performance** (5) | `CacheService`, `CompressionService`, `ConnectionPoolService`, `QueryOptimizerService`, `ResponseOptimizerService` |
| **Communication** (4) | `EmailService`, `SmsService` (mock/twilio/nexmo), `WhatsAppService`, `NotificationService` |
| **Storage** (2) | `FileUploadService`, `CloudinaryService` |
| **Monitoring** (3) | `LoggerService`, `AnalyticsService`, `BackupService` |
| **Other** (6) | `GeolocationService`, `QueueService`, `SearchService`, `WebSocketService`, `LoadingService`, `ToastService`, `ValidationService` |

### CommonModule — Other

| Type | Items |
|------|-------|
| Controllers | `MonitoringController`, `PerformanceController`, `GeocodeController` |
| Middleware | `RequestIdMiddleware`, `CsrfMiddleware` |
| Interceptors | `HttpLoggingInterceptor`, `ResponseTransformInterceptor` |
| Filters | `AllExceptionsFilter` |

### App Controller Routes (outside feature modules)

| Method | Route | Purpose |
|--------|-------|---------|
| GET | `/` | HTML home page |
| GET | `__access/status` | Access gate status |
| POST | `__access/check` | Verify access code |
| POST | `__access/devtools` | DevTools detection → ban |
| GET | `health` | Simple health |
| GET | `health/services` | API + DB + Redis health with latency |
| GET | `health/status` | HTML status page |
| GET | `docs` | API docs page |

### Admin Permissions System

```
ADMIN_DASHBOARD, ADMIN_DRIVERS, ADMIN_USERS, ADMIN_PROVIDERS,
ADMIN_PROMOTIONS, ADMIN_SYSTEM_SETTINGS, ADMIN_HERO_SETTINGS,
ADMIN_ABOUT_IMAGES, ADMIN_COINS_IMAGES, ADMIN_ADD_COINS, ADMIN_CAN_DELETE
```

Super admin: determined by matching `DEFAULT_SUPER_ADMIN_EMAIL` env var.

---

## Web Frontend (Angular 21)

### Tech Stack
- Angular 21 with **standalone components** (no NgModules)
- **Signals** for reactive state
- **OnPush** change detection
- TypeScript 5.9.2
- Vitest 4.0.8 for tests
- Socket.IO Client 4.8.3
- Google Maps JavaScript API + `AdvancedMarkerElement`
- Stripe.js + PayPal
- Font Awesome icons
- CSS Variables (4 themes)
- i18n: 7 languages (EN, FR, LB, DE, IT, PT, ES) + AR for admin

### Routes

**Admin** (`/admin/*`): login, dashboard, profile, users, providers, system-settings, analyst, hero-settings, about-images, coins-images, add-coins, promotions, drivers

**User** (with UserLayout):

| Path | Page |
|------|------|
| `/` | Redirect → `/restaurants` |
| `/home` | Landing page |
| `/restaurants` | Restaurant list (search, categories, offers) |
| `/restaurants/:id` | Restaurant details + menu + add-to-cart |
| `/cart` | Multi-restaurant cart + checkout |
| `/profile` | User profile (info, addresses, payment methods, orders) |
| `/notifications` | Notification center |
| `/orders/:id/tracking` | Live order tracking (Google Maps) |
| `/coins` | Moien Coins wallet page |
| `/about-us` | About page |
| `/pricing` | Pricing page |
| `/accessibility`, `/terms-of-service`, `/privacy-policy` | Legal |
| `/careers`, `/press`, `/business` | Corporate |

**Provider** (`/provider/*`): login → dashboard, analyst-settings, system-settings

**Driver** (`/driver/*`): login/register → dashboard

### 12 Components

`admin-footer`, `admin-navbar`, `cart-sidebar`, `footer`, `global-loading`, `language-modal`, `location-modal`, `navbar`, `partner-location-picker`, `rating-modal`, `theme-modal`, `toast-container`

### 38 Services

| Service | Purpose |
|---------|---------|
| `auth` | Admin JWT login/logout |
| `user-auth` | Customer registration, login, Google OAuth |
| `user-session` | Customer session (localStorage) |
| `partner-auth` | Partner login, localStorage session |
| `driver-auth` | Driver login |
| `token-storage` | JWT token storage |
| `cart-state` | Multi-restaurant cart (signals) |
| `language` | i18n (7 languages, signals) |
| `theme` | 4 CSS themes |
| `restaurants` | Restaurant/category/menu API |
| `orders` | Order placement, tracking |
| `location` | Browser geolocation + Google geocoding |
| `geocoding` | Geocoding API calls |
| `toast` | Toast notifications |
| `loading` | Loading spinner state |
| `site-settings` | CMS content |
| `notifications` | User notifications API |
| `notifications-state` | Notification state (real-time) |
| `partner-places` | Partner dashboard API |
| `partner-notifications` | Admin→Partner messages |
| `ratings` | Reviews API |
| `promotions` | Promo codes API |
| `seo` | Meta tags, structured data |
| `admin-staff` | Admin staff API |
| `admin-dashboard` | Dashboard overview API |
| `admin-analyst` | Analytics API |
| `admin-language` | Admin panel language |
| `admin-master-code` | Master code service |
| `admin-master-code-modal` | Master code modal |
| `admin-permissions` | Permission checks |
| `admin-profile` | Admin profile |
| `admin-provider-analytics` | Provider analytics |
| `admin-provider-notifications` | Admin→provider notifications |
| `provider-system-settings-gate` | Password gate for settings |
| `supportTickets` | Support tickets API |
| `service-status` | API/DB/Redis health |
| `service-status-notifier` | Startup health toast |
| `directif` | Directive utilities |

### 11 Route Guards

| Guard | Purpose |
|-------|---------|
| `authGuard` | Admin JWT auth |
| `loginGuard` | Redirect if admin logged in |
| `adminPermissionGuard` | Permission-based admin access |
| `adminMasterCodeGuard` | Master code protection |
| `userAuthGuard` | Customer JWT auth |
| `partnerAuthGuard` | Partner JWT auth |
| `partnerLoginGuard` | Redirect to dashboard if partner logged in |
| `partnerRedirectGuard` | Keep partner inside /provider/* |
| `driverAuthGuard` | Driver JWT auth |
| `driverRedirectGuard` | Keep driver inside /driver/* |
| `driverNoUserGuard` | Prevent user+driver session conflict |
| `providerSystemSettingsGuard` | System settings password gate |

### 4 HTTP Interceptors

`authInterceptor` (Bearer token), `retryInterceptor` (auto-retry), `loadingInterceptor` (loading state), `errorInterceptor` (toast errors, skip 401/403)

### Provider Dashboard — Key Features

| Feature | Implementation |
|---------|---------------|
| **Short Order IDs** | `shortOrderId(uuid)` → 3 digits + 1 letter (e.g. "012A") |
| **Audio: New Orders** | Loops `order-ringtone.wav` until accepted (`stopOrderAlarm()`) |
| **Audio: Notifications** | Web Audio API single sine tone (520 Hz, 0.5s) |
| **Thermal Print** | Hidden `<iframe>` → 80mm receipt (Courier New, dashed separators, logo, tracking URL) |
| **Invoice Modal** | Customer name/phone/email, items, subtotal, delivery fee, discount, total, address |
| **Order Tracking** | Google Maps `AdvancedMarkerElement` — restaurant (blue) + driver (orange) + polyline, 10s polling |
| **KPI Cards** | Today's orders, revenue, pending, avg prep time, rating (persisted in localStorage) |
| **Sales Analytics** | Hourly (today) + daily (month) breakdown |
| **Menu Management** | Category + item CRUD with addon groups (multi-select), Cloudinary photo upload |
| **Test Orders** | Dev-only: creates fake user + address + cart + cash checkout |
| **Accept/Reject** | Direct order lifecycle control from dashboard |

---

## Database (PostgreSQL 16)

### 16 Entities

| Entity | Table | Key Fields |
|--------|-------|-----------|
| `UserEntity` | `users` | fullName, email, phone, countryCode, **coins** (wallet), stripeCustomerId, passwordHash, passwordResetCodeHash/ExpiresAt, isBanned, photoUrl |
| `UserAddressEntity` | `user_addresses` | userId, label, address fields, lat/lng, isDefault |
| `UserPaymentMethodEntity` | `user_payment_methods` | userId, stripePaymentMethodId, isDefault |
| `AdminStaffEntity` | `admin_staff` | email, passwordHash, permissions[], photoUrl |
| `AuthSessionEntity` | `auth_sessions` | userId, role, refreshTokenHash, expiresAt |
| `RestaurantEntity` | `restaurants` | name, cuisines, address, lat/lng, rating, deliveryFee, isActive, isOpen |
| `RestaurantCategoryEntity` | `restaurant_categories` | restaurantId, name |
| `MenuItemEntity` | `menu_items` | restaurantId, categoryId, name, price, **addons** (JSONB `{name,price,category?,multiSelect?}[]`), isAvailable |
| `OrderEntity` | `orders` | userId, restaurantId, status (cart→placed→accepted→preparing→on_the_way→delivered→canceled), currency, subtotal, deliveryFee, discount, total, driverId, driverAcceptedAt/CanceledAt, paymentMethod, paypalOrderId |
| `OrderItemEntity` | `order_items` | orderId, menuItemId, name, unitPrice, quantity, lineTotal, **selectedAddons** (JSONB) |
| `DriverEntity` | `drivers` | fullName, email, phone, isActive, isOnline, lastLat/Lng |
| `DriverApplicationEntity` | `driver_applications` | fullName, phone, vehicleType |
| `PartnerAccountEntity` | `partner_accounts` | email, passwordHash, restaurantId |
| `PartnerPlaceRequestEntity` | `partner_place_requests` | restaurantId, status (pending/approved/needs_info/rejected), businessName, payout details (IBAN/SWIFT), openFrom/To |
| `PartnerPlaceBranchEntity` | `partner_place_branches` | partnerAccountId, name, address, lat/lng |
| `PromotionEntity` | `promotions` | code (unique), type (percent/fixed), value, minSubtotal, maxDiscount, startsAt/endsAt, usageLimit, perUserLimit |
| `PromotionalOfferEntity` | `promotional_offers` | imageUrl, titleEn/Ar, discountPercent, conditionType (min_order/free_delivery/first_order/min_items), conditionValue, startsAt/endsAt |
| `SiteSettingsEntity` | `site_settings` | key-value CMS |
| `SupportTicketEntity` | `support_tickets` | subject, message, status, replies |
| `NotificationEntity` | `notifications` | userId, title, body, isRead, TTL 10 days |
| `PartnerNotificationEntity` | `partner_notifications` | restaurantId, title, body, isRead |
| `PlatformRatingEntity` | `platform_ratings` | userId, rating, comment |
| `RestaurantRatingEntity` | `restaurant_ratings` | userId, restaurantId, orderId, rating, comment |

### 25 Migrations (Feb 7 – Mar 10, 2026)

| Date | Migration |
|------|-----------|
| 2026-02-07 | create-admin-staff |
| 2026-02-07 | soft-delete-and-indexes |
| 2026-02-07 | auth-sessions |
| 2026-02-09 | partner-places |
| 2026-02-10 | partner-places-image-url |
| 2026-02-12 | partner-place-hours |
| 2026-02-12 | partner-place-branches |
| 2026-02-12 | partner-place-restaurant-link |
| 2026-02-14 | promotion-restaurants |
| 2026-02-16 | partner-place-description |
| 2026-02-16 | ratings |
| 2026-02-17 | partner-place-payout-details |
| 2026-02-17 | stripe-connect-account |
| 2026-02-17 | remove-stripe-connect-account |
| 2026-02-17 | admin-staff-permissions |
| 2026-02-17 | default-super-admin |
| 2026-02-17 | ensure-default-super-admin-permissions |
| 2026-02-17 | drivers-and-dispatch |
| 2026-02-17 | partner-place-branch-coordinates |
| 2026-02-18 | partner-notifications |
| 2026-02-20 | driver-order-lifecycle |
| 2026-02-27 | promotional-offers |
| 2026-02-28 | driver-applications |
| 2026-03-01 | menu-item-addons |
| 2026-03-10 | user-coins |
| 2026-03-10 | password-reset-fields |

---

## Real-Time (Socket.IO 4.8)

### 8 WebSocket Gateways

| Namespace | Auth | Events (Emit) | Purpose |
|-----------|------|--------------|---------|
| `/users` | Admin JWT | `users.created`, `users.updated`, `users.deleted`, `account.banned` | Admin user management |
| `/notifications` | User JWT | `notifications.created`, `notifications.read`, `notifications.readAll` | Customer notifications |
| `/site-settings` | None | `hero-image.updated`, `landing-image.updated`, `landing-images.updated`, `about-image.updated`, `about-images.updated` | CMS live updates |
| `/partner-places` | Admin JWT | `partner-places.changed` (kinds: created, approved, needs_info, rejected, deleted, updated) | Admin: restaurant status |
| `/admin-drivers` | Admin JWT | `adminDrivers.snapshot` (10s polling) | Admin: drivers locations |
| `/driver` | Driver JWT | `driver.orders`, `driver.orderAssigned`, `driver.message`, `driver.forcedLogout`, `driver.inbox` | Driver dispatch & messaging |
| `/user-orders` | User JWT | `userOrders.orders`, `userOrders.order.tracking` | Customer order tracking |
| `/partner-orders` | Partner JWT | `partner-orders.created`, `partner-orders.updated` | Restaurant: incoming orders |
| `/partner-notifications` | Partner JWT | `partner-notifications.created` | Admin messages to restaurant |

### Driver Gateway — Listen Events

`driver.subscribe`, `driver.online.set`, `driver.location.update`, `driver.order.accept`, `driver.order.cancel`, `driver.order.delivered`, `driver.inbox.get`, `driver.inbox.clear`, `driver.inbox.delete`

### Admin Drivers Gateway — Listen Events

`adminDrivers.subscribe`, `adminDrivers.driver.setActive`, `adminDrivers.driver.sendMessage`

### User Orders Gateway — Listen Events

`userOrders.subscribe`, `userOrders.order.subscribe`, `userOrders.order.unsubscribe`

---

## Security & Auth

### Authentication Flows

| Flow | Method | Token |
|------|--------|-------|
| Customer login | Email/password or Google OAuth | JWT access (7d) + refresh (365d) |
| Admin login | Email/password | JWT access (7d) + refresh (365d) |
| Driver login | Email/password | JWT access (7d) + refresh (365d) |
| Partner login | Email/password | JWT partner (30d) |
| Password reset | Email or SMS channel | Reset code (time-limited) |

### Security Layers

1. **Transport** — HTTPS/TLS via Nginx + Let's Encrypt
2. **Network** — UFW: only ports 22, 80, 443; PostgreSQL on localhost only; port 5432 denied
3. **API** — Helmet headers, CORS whitelist, rate limiting (`ThrottlerGuard` global), CSRF middleware (double-submit cookie), request ID middleware
4. **Validation** — `ValidationPipe` with whitelist + forbidNonWhitelisted + transform
5. **Auth** — JWT with refresh rotation, bcrypt password hashing, session tracking, `ServerAccessService` (access gate with lockout)
6. **Permissions** — Role-based guards + permission-based admin access (11 permissions)
7. **Anti-abuse** — DevTools detection → 10 min ban, unusual activity → 1 hour ban, access code lockout (3 attempts → 5 min)

---

## External Integrations

| Service | Package | Purpose | Config |
|---------|---------|---------|--------|
| **Stripe** | stripe 17.7.0 | Card payments, webhooks, payment methods, **wallet top-up** | `STRIPE_SECRET_KEY`, `STRIPE_PUBLISHABLE_KEY`, `STRIPE_WEBHOOK_SECRET` |
| **PayPal** | REST API v2 | Alternative payments (live by default) | `PAYPAL_CLIENT_ID`, `PAYPAL_SECRET` |
| **Twilio** | twilio 5.12.1 | SMS/WhatsApp (order confirmations, OTP) | `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER` |
| **Google Maps** | JS API | Geocoding, maps, AdvancedMarkerElement | `GOOGLE_MAPS_API_KEY` |
| **Google OAuth** | tokeninfo endpoint | Social login | `GOOGLE_OAUTH_CLIENT_ID` |
| **Cloudinary** | cloudinary 2.9.0 | Image upload & CDN | `CLOUDINARY_URL` |
| **Nodemailer** | nodemailer | SMTP email (branded HTML templates) | `MAIL_HOST`, `MAIL_USER`, `MAIL_PASS` |
| **Redis** | ioredis 5.9.2 | Cache + Bull queues (fallback: in-memory) | `REDIS_URL` |
| **ExcelJS** | exceljs 4.4.0 | Excel export for analytics | — |

---

## Deployment Architecture

### Production Stack

```
Hostinger VPS (Ubuntu) — api.moiendelivery.lu / moiendelivery.lu
├── Nginx       → SSL (Let's Encrypt), SPA routing, reverse proxy to :3000, WebSocket upgrade
├── PM2         → Process manager for NestJS (fork mode, auto-restart)
├── PostgreSQL  → localhost only (listen_addresses = 'localhost')
├── Redis       → Cache + job queues
└── UFW         → 22, 80, 443 ALLOW; 5432 DENY
```

### Directory Layout

```
/home/moiendelivery/htdocs/moiendelivery.lu           ← Angular dist (static)
/home/moiendelivery-api/htdocs/api.moiendelivery.lu   ← NestJS dist + node_modules + .env
```

### PM2 Config (ops/pm2/ecosystem.config.cjs)

- App name: `moiendelivery-api`
- Script: `dist/main.js`
- Mode: fork
- PORT: 3000
