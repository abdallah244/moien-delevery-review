# Moien Delivery — Service Catalog

> Last updated: March 2026 · v0.0.17+

---

## Principles

- This file documents **what actually exists** in the codebase (modules, services, routes).
- For endpoint details (request/response): see [docs/API.md](../docs/API.md).
- For real-time events (Socket.IO): see [docs/WEBSOCKETS.md](../docs/WEBSOCKETS.md).

---

## URLs

| Environment      | URL                                   |
| ---------------- | ------------------------------------- |
| API (production) | `https://api.moiendelivery.lu/api/v1` |
| Web (production) | `https://moiendelivery.lu`            |
| API (local)      | `http://localhost:3000/api/v1`        |
| Web (local)      | `http://localhost:4200`               |

---

## Backend Modules (13)

| Module          | Base Route         | Key Endpoints                                                                                                                                             | Notes                                                                                       |
| --------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Auth            | `/auth`            | `POST /login`, `POST /refresh`, `POST /logout`, `GET /sessions`, `POST /user/google`, `POST /forgot-password/*`                                           | JWT (User/Admin/Driver) + refresh rotation + Google OAuth + forgot-password (3-step)        |
| Users           | `/users`           | CRUD, `/me/profile`, `/verify-email`, `/verify-phone`, `/addresses`, `/payment-methods`, `/:id/add-coins`                                                 | Email/Phone OTP, Stripe PMs, **Moien Coins wallet**                                         |
| Restaurants     | `/restaurants`     | CRUD, search, filters                                                                                                                                     | + `/restaurant-categories` + `/menu-items` (with **addons** JSONB)                          |
| Orders          | `/orders`          | `POST /cart`, `/checkout`, `/checkout-session`, `/paypal/*`, **`/wallet/checkout`**, **`/wallet/topup`**, `GET /me/carts`, `GET /:id/tracking`            | Multi-cart, Stripe, PayPal, Cash, **Moien Coins**, cashback, auto-apply offers, short IDs   |
| Drivers         | `/drivers`         | `/me`, `/online`, `/location`, `/orders`, **`/apply`**                                                                                                    | + `/admin/drivers` CRUD + **driver applications**                                           |
| Payments        | `/payments`        | `GET /stripe/publishable-key`, **`GET /paypal/client-id`**, `POST /stripe/webhook`                                                                        | Stripe + PayPal + Cash on delivery                                                          |
| Promotions      | `/promotions`      | CRUD, `POST /validate`, `POST /:id/notify`                                                                                                                | + **`/promotional-offers`** (visual banners with auto-apply)                                |
| Ratings         | `/ratings`         | `POST /restaurant`, `GET /platform`, **`GET /partner/me/summary`**                                                                                        | Restaurant + platform + **partner ratings**                                                 |
| Notifications   | `/notifications`   | `GET /`, `PATCH /:id/read`, `POST /read-all`, `DELETE /`, `DELETE /:id`                                                                                   | DB + Socket.IO real-time, TTL cleanup                                                       |
| Partner Places  | `/partner-places`  | `/me`, `/request`, `/menu-items`, `/categories`, branches, **`/orders/:id/accept`**, **`/orders/:id/reject`**, **`/analytics/sales`**, **`/orders/test`** | Full lifecycle, Cloudinary, **accept/reject**, **analytics**, **test orders**               |
| Site Settings   | `/site-settings`   | hero/landing/about/**coins** image endpoints, `GET/POST/DELETE /:key`                                                                                     | CMS images + general settings                                                               |
| Support Tickets | `/support-tickets` | `POST /`, `GET /`, `POST /:id/reply`, `POST /:id/open`, `DELETE /:id`                                                                                     | Customer support                                                                            |
| Admin Staff     | `/admin-staff`     | CRUD, login, `/dashboard/overview`, `/analyst/*`, **`/provider-analyst/*`** (XLSX export), `/provider-notifications/send`                                 | 6 controllers: staff, profile, dashboard, analyst, provider-analyst, provider-notifications |

### Additional Routes

| Route                    | Module               | Notes                                                        |
| ------------------------ | -------------------- | ------------------------------------------------------------ |
| `/geocode`               | Common               | Nominatim proxy (reverse + search) — no auth required        |
| `/monitoring/logs`       | Common               | Structured logs + error logs + stats (env gated)             |
| `/performance/*`         | Common               | Cache, compression, pool, query/response optimizer endpoints |
| `/health/services`       | App                  | API/DB/Redis health check with latency                       |
| `/partner-notifications` | PartnerNotifications | `GET /` + `DELETE /:id` (PartnerJwtAuthGuard)                |

---

## CommonModule — 26 Shared Services

| Category      | Services                                                                                                                 | Purpose                                                                                |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| Security      | `EncryptionService`, `IpGuardService`, `JwtAuthService`, `RateLimiterService`, `SanitizerService`, `ServerAccessService` | AES-256 encryption, bcrypt, JWT management, rate limiting, XSS prevention, IP tracking |
| Performance   | `CacheService`, `CompressionService`, `ConnectionPoolService`, `QueryOptimizerService`, `ResponseOptimizerService`       | Redis/in-memory cache, gzip/brotli, DB pooling, query optimization                     |
| Communication | `EmailService`, `SmsService`, `NotificationService`, `WhatsappService`                                                   | Nodemailer SMTP, Twilio SMS, push notifications, WhatsApp messaging                    |
| Storage       | `FileUploadService`, `CloudinaryService`                                                                                 | Local file upload, Cloudinary CDN                                                      |
| Monitoring    | `LoggerService`                                                                                                          | Structured logging                                                                     |
| Queue         | `QueueService`                                                                                                           | Bull + Redis background jobs                                                           |
| Realtime      | `WebsocketService`                                                                                                       | Socket.IO helpers                                                                      |
| Search        | `SearchService`                                                                                                          | Full-text search                                                                       |
| Analytics     | `AnalyticsService`                                                                                                       | Business analytics                                                                     |
| Backup        | `BackupService`                                                                                                          | Database backup/restore                                                                |
| Location      | `GeolocationService`                                                                                                     | Distance calculation, delivery zones                                                   |
| UI            | `LoadingService`, `ToastService`, `ValidationService`                                                                    | Server-side UI helpers                                                                 |
| Payment       | `PaypalService`, `StripeService`                                                                                         | PayPal REST v2, Stripe SDK                                                             |
| Database      | `DatabaseHealthService`                                                                                                  | DB health checks                                                                       |

### Common Controllers (3)

| Controller              | Route          | Purpose                                       |
| ----------------------- | -------------- | --------------------------------------------- |
| `GeocodeController`     | `/geocode`     | Nominatim proxy (reverse + search)            |
| `MonitoringController`  | `/monitoring`  | Logs + errors + stats                         |
| `PerformanceController` | `/performance` | Cache, compression, pool, optimizer endpoints |

---

## WebSocket Namespaces (9)

| Namespace                | Auth        | Key Events                                                                                       | Purpose                      |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------------ | ---------------------------- |
| `/users`                 | Admin JWT   | `users.created`, `users.updated`, `users.deleted`, `account.banned`                              | Admin user management        |
| `/notifications`         | User JWT    | `notifications.created`, `notifications.read`, `notifications.readAll`                           | Customer notifications       |
| `/site-settings`         | None        | `hero-image.updated`, `landing-image(s).updated`, `about-image(s).updated`                       | CMS live updates             |
| `/partner-places`        | Admin JWT   | `partner-places.changed`                                                                         | Admin: restaurant status     |
| `/admin-drivers`         | Admin JWT   | `adminDrivers.snapshot` (10s interval)                                                           | Admin: driver locations      |
| `/driver`                | Driver JWT  | `driver.orders`, `driver.orderAssigned`, `driver.message`, `driver.inbox`, `driver.forcedLogout` | Driver dispatch & messaging  |
| `/user-orders`           | User JWT    | `userOrders.orders`, `userOrders.order.tracking`                                                 | Customer order tracking      |
| `/partner-orders`        | Partner JWT | `partner-orders.created`, `partner-orders.updated`                                               | Restaurant incoming orders   |
| `/partner-notifications` | Partner JWT | `partner-notifications.created`                                                                  | Admin messages to restaurant |

---

## Frontend Services (38)

| Service                       | Purpose                                |
| ----------------------------- | -------------------------------------- |
| `AuthService`                 | Admin JWT login/logout                 |
| `UserAuthService`             | Customer registration, login, refresh  |
| `UserSessionService`          | Customer session (localStorage)        |
| `PartnerAuthService`          | Partner login, localStorage session    |
| `DriverAuthService`           | Driver login                           |
| `TokenStorageService`         | JWT token storage                      |
| `CartStateService`            | Multi-restaurant cart (signals)        |
| `LanguageService`             | i18n (7 languages, signals)            |
| `ThemeService`                | 4 CSS themes                           |
| `RestaurantsService`          | Restaurant/category/menu API           |
| `OrdersService`               | Order placement, tracking, **wallet**  |
| `LocationService`             | Browser geolocation + Google geocoding |
| `ToastService`                | Toast notifications                    |
| `SiteSettingsService`         | CMS content from backend               |
| `NotificationsService`        | User notifications API                 |
| `NotificationsStateService`   | Notification state (real-time)         |
| `PartnerPlacesService`        | Partner dashboard API                  |
| `PartnerNotificationsService` | Admin→Partner messages                 |
| `RatingsService`              | Reviews API                            |
| `PromotionsService`           | Promo codes API                        |
| `PromotionalOffersService`    | Visual offer banners                   |
| `SeoService`                  | Meta tags, structured data             |
| `AdminStaffService`           | Admin staff API                        |
| `AdminLanguageService`        | Admin panel language                   |
| `AdminMasterCodeService`      | Admin master code                      |
| `SupportTicketsService`       | Support tickets API                    |
| `LoadingService`              | Loading spinner state                  |
| `PaymentMethodsService`       | Stripe payment methods                 |
| `DriversService`              | Drivers admin API                      |
| `ProviderAnalyticsService`    | Provider analytics + XLSX export       |

---

## Frontend Guards (11)

| Guard                         | Purpose                                    |
| ----------------------------- | ------------------------------------------ |
| `authGuard`                   | Admin JWT auth                             |
| `loginGuard`                  | Redirect if admin logged in                |
| `adminPermissionGuard`        | Permission-based admin access              |
| `adminMasterCodeGuard`        | Master code protection                     |
| `userAuthGuard`               | Customer JWT auth                          |
| `partnerAuthGuard`            | Partner JWT auth                           |
| `partnerLoginGuard`           | Redirect to dashboard if partner logged in |
| `partnerRedirectGuard`        | Keep partner inside /provider/\*           |
| `driverAuthGuard`             | Driver JWT auth                            |
| `driverRedirectGuard`         | Keep driver inside /driver/\*              |
| `driverNoUserGuard`           | Prevent user+driver session conflict       |
| `providerSystemSettingsGuard` | Password-protected provider settings       |
