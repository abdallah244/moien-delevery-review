<div align="center">

# Moien Delivery

### Food Delivery Platform — Luxembourg

[![NestJS](https://img.shields.io/badge/NestJS-11-ea2845.svg)](https://nestjs.com/)
[![Angular](https://img.shields.io/badge/Angular-21-dd0031.svg)](https://angular.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178c6.svg)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791.svg)]()
[![License](https://img.shields.io/badge/License-MIT-orange.svg)](LICENSE)

</div>

---

## Overview

Moien Delivery is a full-stack food delivery platform for Luxembourg with four user roles: **customers**, **restaurant partners**, **delivery drivers**, and **admins**.

| Component      | Stack                                      | Status         |
| -------------- | ------------------------------------------ | -------------- |
| Backend API    | NestJS 11, TypeORM 0.3.28, PostgreSQL 16   | ✅ Production  |
| Web Frontend   | Angular 21, Standalone Components, Signals | ✅ Production  |
| Mobile App     | Flutter (planned)                          | ❌ Not started |
| Infrastructure | Hostinger VPS, Nginx, PM2, UFW             | ✅ Production  |

**Live:** [moiendelivery.lu](https://moiendelivery.lu) · **API:** [api.moiendelivery.lu](https://api.moiendelivery.lu)

---

## Quick Start

```bash
# Install (npm workspaces)
npm install

# Backend
cd backend
cp .env.example .env          # Fill DB credentials, JWT secrets
npm run db:migrate:run         # 25 migrations
npm run db:seed                # Optional sample data
npm run start:dev              # http://localhost:3000

# Frontend (new terminal)
cd wfrontend
npm start                      # http://localhost:4200
```

**Admin:** `admin@moien.com` / `123456`

---

## Tech Stack

| Layer       | Technology                                          | Version               |
| ----------- | --------------------------------------------------- | --------------------- |
| Backend     | NestJS + TypeORM                                    | 11.x / 0.3.28         |
| Frontend    | Angular (standalone, signals, OnPush)               | 21.0.0                |
| Database    | PostgreSQL                                          | 16                    |
| Cache/Queue | Redis (ioredis) + Bull                              | 5.9.2                 |
| Realtime    | Socket.IO                                           | 4.8.3                 |
| Payments    | Stripe + PayPal                                     | 17.7.0 / REST v2      |
| SMS         | Twilio                                              | 5.12.1                |
| Images      | Cloudinary                                          | 2.9.0                 |
| Maps        | Google Maps JS API                                  | AdvancedMarkerElement |
| Auth        | JWT (bcrypt) + Google OAuth                         | —                     |
| i18n        | 7 languages (EN, FR, LB, DE, IT, PT, ES) + AR admin | —                     |
| Themes      | 4 CSS Variable themes                               | —                     |
| Testing     | Jest 30 (backend) / Vitest 4 (frontend)             | —                     |
| Export      | ExcelJS                                             | 4.4.0                 |

---

## Project Structure

```
moien-delivery/
├── package.json                  # npm workspaces ["backend", "wfrontend"]
│
├── backend/                      # NestJS 11 API
│   ├── src/
│   │   ├── main.ts               # rawBody, CORS, Helmet, ValidationPipe
│   │   ├── app.module.ts         # 13 feature modules + CommonModule + ThrottlerGuard
│   │   ├── app.controller.ts     # Health checks, access gate, docs
│   │   ├── config/               # app, database, jwt, stripe, paypal, cloudinary
│   │   ├── common/               # 26 services, 3 controllers, middleware, interceptors, filters
│   │   ├── modules/
│   │   │   ├── admin-staff/      # 6 controllers, analytics gateway
│   │   │   ├── auth/             # User/Admin/Driver JWT, Google OAuth, forgot-password
│   │   │   ├── users/            # CRUD, addresses, Stripe PMs, Moien Coins, OTP
│   │   │   ├── restaurants/      # Restaurants, categories, menu items (addons)
│   │   │   ├── orders/           # Cart, checkout (cash/Stripe/PayPal/Coins), tracking
│   │   │   ├── drivers/          # Registration, applications, dispatch, messaging
│   │   │   ├── payments/         # Stripe webhook, PayPal client-id
│   │   │   ├── promotions/       # Promo codes + promotional offer banners
│   │   │   ├── ratings/          # Platform + restaurant reviews
│   │   │   ├── notifications/    # User notifications (DB + Socket.IO)
│   │   │   ├── partner-places/   # Partner lifecycle, orders, analytics, menu CRUD
│   │   │   ├── site-settings/    # Landing CMS (hero, about, coins images)
│   │   │   └── support-tickets/  # Customer support
│   │   └── database/
│   │       ├── migrations/       # 25 migrations (Feb 7 – Mar 10, 2026)
│   │       ├── data-source.ts
│   │       └── seed.ts
│   └── test/
│
├── wfrontend/                    # Angular 21
│   ├── src/app/
│   │   ├── pages/
│   │   │   ├── admin/            # 13 sub-pages
│   │   │   ├── provider/         # login, dashboard, analyst-settings, system-settings
│   │   │   ├── driver/           # login/register, dashboard
│   │   │   ├── landing/          # Marketing page
│   │   │   ├── restaurants/      # List + details
│   │   │   ├── cart/             # Cart + checkout
│   │   │   └── user/             # Profile, orders, notifications, coins, about, legal
│   │   ├── services/             # 38 services
│   │   ├── guards/               # 11 route guards
│   │   ├── interceptors/         # 4 HTTP interceptors
│   │   ├── components/           # 12 shared components
│   │   └── layouts/              # user-layout, admin-layout
│   └── src/environments/
│
├── docs/                         # Technical docs
├── markdown/                     # Project-level docs
├── ops/                          # Nginx + PM2 configs
└── uploads/
```

---

## Key Features

### Customers

- Browse restaurants (search, category carousel, promotional offer banners)
- Multi-restaurant cart, per-restaurant checkout
- **4 payment methods:** Stripe, PayPal, Cash on delivery, **Moien Coins wallet**
- Moien Coins: top-up via Stripe, **1% cashback** on every order, admin can add coins
- **Free delivery for first 30 days** after registration
- **Promotional offers auto-applied** at checkout (best matching offer)
- Promo codes (percent/fixed, start/end dates, per-user limits)
- Real-time order tracking (Google Maps AdvancedMarkerElement, 10s polling)
- Short order IDs (e.g. "012A") for easy reference
- Email + SMS order confirmation with tracking URL
- Notification center (real-time via Socket.IO, 10-day TTL)
- Password reset (3-step: email or SMS)
- 7 languages, 4 themes

### Restaurant Partners

- Partner onboarding workflow (request → approve/reject)
- Dashboard: KPI cards (orders, revenue, pending, avg prep time, rating)
- **Sales analytics:** hourly (today) + daily (month) breakdown
- Accept/reject orders directly from dashboard
- Menu management: categories + items with **addon groups** (multi-select, price per addon)
- **Audio alerts:** `order-ringtone.wav` loops until accepted; Web Audio sine tone for admin messages
- **Thermal receipt printing** (80mm, hidden iframe, Courier New monospace)
- Invoice modal: logo, customer details, items, totals, delivery address, tracking URL
- Live order tracking (Google Maps, driver location)
- Cloudinary image upload for restaurant + menu items
- Branches management
- System settings (password-protected)
- **Test orders** (dev environment only)

### Drivers

- Driver application system (`POST drivers/apply`)
- Login + dashboard (online/offline toggle, GPS location)
- Order dispatch: nearest available driver (Haversine distance)
- Order lifecycle: accept → pick up → deliver (with cancel + reason)
- Admin messaging: Redis-backed inbox with 3-day TTL
- Real-time updates via Socket.IO

### Admins

- Dashboard: counts (restaurants, orders, users, drivers), pending actions
- User management: CRUD, ban/unban, **add Moien Coins**
- **11 permissions:** ADMIN_DASHBOARD, ADMIN_USERS, ADMIN_DRIVERS, ADMIN_PROVIDERS, ADMIN_PROMOTIONS, ADMIN_SYSTEM_SETTINGS, ADMIN_HERO_SETTINGS, ADMIN_ABOUT_IMAGES, ADMIN_COINS_IMAGES, ADMIN_ADD_COINS, ADMIN_CAN_DELETE
- Restaurant partner lifecycle (approve, reject, needs_info, message, delete)
- Driver management (create, activate/deactivate, force logout, message)
- Promotions: promo codes + promotional offer banners (bilingual, time-limited, condition-based)
- CMS: hero images, landing images, about images, **coins images**
- Analytics dashboard
- Support ticket management
- Real-time WebSocket updates

---

## API Modules (13)

| Module         | Base Route         | Entities                                                | Key Features                                                                                      |
| -------------- | ------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Auth           | `/auth`            | AuthSession                                             | User/Admin/Driver JWT + refresh, Google OAuth, forgot-password (3-step)                           |
| Users          | `/users`           | User, UserAddress, UserPaymentMethod                    | CRUD, addresses, Stripe PMs, OTP, **Moien Coins**, ban                                            |
| Restaurants    | `/restaurants`     | Restaurant, RestaurantCategory, MenuItem                | CRUD, search/filter, categories, items with **addons** (JSONB)                                    |
| Orders         | `/orders`          | Order, OrderItem                                        | Cart, checkout (cash/Stripe/PayPal/**Coins**), tracking, **cashback**, **auto-offers**, email+SMS |
| Drivers        | `/drivers`         | Driver, DriverApplication                               | CRUD, applications, online/location, dispatch, order lifecycle                                    |
| Payments       | `/payments`        | —                                                       | Stripe key, Stripe webhook, PayPal client-id                                                      |
| Promotions     | `/promotions`      | Promotion, PromotionalOffer, + join tables              | Codes validate/redeem, offer banners, auto-apply                                                  |
| Ratings        | `/ratings`         | PlatformRating, RestaurantRating                        | Restaurant + platform reviews                                                                     |
| Notifications  | `/notifications`   | Notification                                            | User notifications, 10-day TTL                                                                    |
| PartnerPlaces  | `/partner-places`  | PartnerAccount, PartnerPlaceRequest, PartnerPlaceBranch | Full lifecycle, menu CRUD, **orders accept/reject**, **sales analytics**, branches                |
| SiteSettings   | `/site-settings`   | SiteSettings                                            | CMS (hero, landing, about, **coins** images)                                                      |
| SupportTickets | `/support-tickets` | SupportTicket                                           | Public create, admin manage                                                                       |
| AdminStaff     | `/admin-staff`     | AdminStaff                                              | 6 controllers, dashboard overview, analytics                                                      |

---

## WebSocket (8 Namespaces)

| Namespace                | Auth    | Key Events                                                                       |
| ------------------------ | ------- | -------------------------------------------------------------------------------- |
| `/users`                 | Admin   | `users.created/updated/deleted`, `account.banned`                                |
| `/notifications`         | User    | `notifications.created/read/readAll`                                             |
| `/site-settings`         | None    | `hero-image.updated`, `landing/about-image(s).updated`                           |
| `/partner-places`        | Admin   | `partner-places.changed`                                                         |
| `/admin-drivers`         | Admin   | `adminDrivers.snapshot` (10s)                                                    |
| `/driver`                | Driver  | `driver.orders`, `driver.orderAssigned`, `driver.message`, `driver.forcedLogout` |
| `/user-orders`           | User    | `userOrders.orders`, `userOrders.order.tracking`                                 |
| `/partner-orders`        | Partner | `partner-orders.created/updated`                                                 |
| `/partner-notifications` | Partner | `partner-notifications.created`                                                  |

---

## Database (25 Migrations, 16 Entities)

Migrations cover: admin staff, auth sessions, partner system (requests, branches, hours, payouts, descriptions), restaurants (with addons), orders + items, promotions (codes + offers), ratings, drivers (+ applications + dispatch + lifecycle), notifications (user + partner), site settings, **Moien Coins** (user wallet), **password reset fields**.

---

## Deployment

```
Hostinger VPS (Ubuntu)
├── Nginx       → SSL, SPA routing, proxy :3000, WebSocket upgrade
├── PM2         → NestJS process manager (moiendelivery-api, fork mode)
├── PostgreSQL  → localhost only
├── Redis       → Cache + queues
└── UFW         → 22/80/443 ALLOW, 5432 DENY
```

**Deploy scripts:** `c:\tmp\deploy_now.py` (frontend), `c:\tmp\deploy_backend.py` (backend), `c:\tmp\deploy_all.py` (both), `c:\tmp\run_migration.py` (migrations)

---

## Documentation

| Doc                                            | Content                                              |
| ---------------------------------------------- | ---------------------------------------------------- |
| [ARCHITECTURE.md](markdown/ARCHITECTURE.md)    | System architecture, all modules, entities, gateways |
| [docs/API.md](docs/API.md)                     | REST API endpoints reference                         |
| [docs/DATABASE.md](docs/DATABASE.md)           | Schema, ERD, migrations                              |
| [docs/WEBSOCKETS.md](docs/WEBSOCKETS.md)       | Socket.IO namespaces & events                        |
| [docs/FRONTEND-WEB.md](docs/FRONTEND-WEB.md)   | Angular frontend docs                                |
| [docs/SETUP.md](docs/SETUP.md)                 | Environment setup guide                              |
| [docs/HOSTINGER-VPS.md](docs/HOSTINGER-VPS.md) | VPS deployment & security                            |
| [docs/i18n.md](docs/i18n.md)                   | Internationalization                                 |
| [markdown/CATALOG.md](markdown/CATALOG.md)     | Service catalog                                      |
| [markdown/REPORT.md](markdown/REPORT.md)       | Project status report                                |
| [markdown/CHANGELOG.md](markdown/CHANGELOG.md) | Version history                                      |

---

## License

MIT — see [LICENSE](LICENSE)
