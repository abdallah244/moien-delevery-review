# 📊 تقرير حالة المشروع | Project Status Report

<div align="center">

[![Version](https://img.shields.io/badge/Version-v0.0.17-orange.svg)]()
[![Status](https://img.shields.io/badge/Status-قيد%20التطوير-yellow.svg)]()
[![Last Updated](https://img.shields.io/badge/آخر%20تحديث-يوليو%202026-orange.svg)]()

**📅 تاريخ التقرير: يوليو 2026**

</div>

---

## 📋 جدول المحتويات

- [ملخص تنفيذي](#-ملخص-تنفيذي)
- [جرد المشروع (Inventory)](#-جرد-المشروع-inventory)
- [المشاكل الحرجة](#-المشاكل-الحرجة-critical-issues)
- [الأشياء التي تحتاج إصلاح](#-الأشياء-التي-تحتاج-إصلاح-bugs--fixes)
- [الأشياء التي تحتاج تحسين](#-الأشياء-التي-تحتاج-تحسين-improvements)
- [متغيرات البيئة (.env) وتشغيل المشروع](#-متغيرات-البيئة-env-وتشغيل-المشروع)
- [أجزاء موجودة لكن غير مستخدمة/غير مكتملة](#-أجزاء-موجودة-لكن-غير-مستخدمةغير-مكتملة)
- [الميزات الناقصة](#-الميزات-الناقصة-missing-features)
- [مميزات تميزنا عن المنافسين](#-مميزات-تميزنا-عن-المنافسين-competitive-advantages)
- [خريطة الطريق المقترحة](#-خريطة-الطريق-المقترحة)
- [آخر التحديثات](#-آخر-التحديثات-v0015)

---

## 📈 ملخص تنفيذي

### نسبة الإنجاز الحالية

| المكون           | نسبة الإنجاز | الحالة           |
| ---------------- | ------------ | ---------------- |
| 🖥️ Backend API   | 98%          | 🟢 مكتمل تقريباً |
| 🌐 Web Frontend  | 95%          | 🟢 مكتمل تقريباً |
| 📱 Mobile App    | 0%           | 🔴 لم يبدأ       |
| 🗄️ Database      | 95%          | 🟢 مكتمل تقريباً |
| 📚 Documentation | 95%          | 🟢 جيد           |
| 🧪 Testing       | 18%          | 🔴 ضعيف          |

### ما تم إنجازه ✅

| المكون   | التفاصيل                                                                                                                                                                                                                                 |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Backend  | 13 وحدة فعلياً في `backend/src/modules`: Admin Staff / Users / Auth / Payments / Support Tickets / Site Settings / Restaurants / Orders / Promotions / Notifications / Partner Places / Drivers / Ratings                                |
| Backend  | Restaurants + Restaurant Categories + Menu Items (CRUD + filtering)                                                                                                                                                                      |
| Backend  | Orders (Cart) + Multi-Restaurant Cart (`GET /me/carts`) + `enrichCartWithRestaurant()` + Checkout (promoCode + discount) + Tracking endpoint للمستخدم                                                                                    |
| Backend  | Promotions (Validate + Redemption tracking)                                                                                                                                                                                              |
| Backend  | Notifications (DB + Socket.IO realtime عبر namespace /notifications)                                                                                                                                                                     |
| Backend  | CommonModule: **26 خدمة مشتركة** + 3 controllers (Geocode/Monitoring/Performance) + Redis cache اختياري عبر env                                                                                                                          |
| Backend  | إعداد PostgreSQL + TypeORM                                                                                                                                                                                                               |
| Backend  | تكامل Stripe + Cloudinary + **PayPal** + **Moien Coins wallet**                                                                                                                                                                          |
| Backend  | **Moien Coins**: walletCheckout, walletTopUp, 1% cashback على كل طلب, admin add-coins, Stripe top-up                                                                                                                                     |
| Backend  | **Password Reset**: 3 خطوات (request → verify → reset) عبر email أو SMS                                                                                                                                                                  |
| Backend  | **Google OAuth**: `POST auth/user/google` (Throttle 10/60s)                                                                                                                                                                              |
| Backend  | **Menu Item Addons**: JSONB addon groups مع multiSelect + حساب الأسعار + selectedAddons في OrderItem                                                                                                                                     |
| Backend  | **Partner Accept/Reject**: `POST partner-places/me/orders/:id/accept                                                                                                                                                                     | reject` |
| Backend  | **Partner Sales Analytics**: `GET partner-places/me/analytics/sales` (ساعي + يومي)                                                                                                                                                       |
| Backend  | **Driver Applications**: `POST drivers/apply` (عام)                                                                                                                                                                                      |
| Backend  | **Promotional Offers**: auto-apply أفضل عرض عند الدفع + توصيل مجاني للمستخدمين الجدد (30 يوم)                                                                                                                                            |
| Backend  | **Test Orders**: `POST partner-places/me/orders/test` (بيئة التطوير فقط)                                                                                                                                                                 |
| Backend  | **Admin Permissions**: 11 صلاحية (ADMIN_DASHBOARD, ADMIN_USERS, ADMIN_DRIVERS, ADMIN_PROVIDERS, ADMIN_PROMOTIONS, ADMIN_SYSTEM_SETTINGS, ADMIN_HERO_SETTINGS, ADMIN_ABOUT_IMAGES, ADMIN_COINS_IMAGES, ADMIN_ADD_COINS, ADMIN_CAN_DELETE) |
| Backend  | **Admin Analytics**: revenue (daily/monthly/yearly) + staff presence + staff sessions + provider analytics + XLSX export                                                                                                                 |
| Backend  | **26 Common Services** + 3 controllers (الجيوكود + المراقبة + الأداء)                                                                                                                                                                    |
| Backend  | **26 Migrations** (Feb 7 – Mar 10, 2026) شاملة user-coins + password-reset-fields + menu-item-addons + driver-applications + promotional-offers                                                                                          |
| Backend  | Drivers (MVP): CRUD للأدمن + login للسائق + Online/Location + Realtime عبر Socket.IO + إسناد أقرب سائق للطلب (best-effort)                                                                                                               |
| Backend  | Drivers (Updates): deactivate/activate من الأدمن + force logout + رسائل أدمن→سائق (realtime + inbox persistent best-effort) + Driver order lifecycle (accept/cancel/delivered)                                                           |
| Backend  | Service Health: `GET /api/v1/health/services` لفحص API/DB/Redis (latency + سبب الخطأ)                                                                                                                                                    |
| Frontend | Angular Standalone + Interceptors + خدمات Session/Toast/Language + تكامل Socket.IO في صفحات محددة                                                                                                                                        |
| Frontend | نظام المصادقة + لوحة تحكم الأدمن                                                                                                                                                                                                         |
| Frontend | صفحة الهبوط (Landing Page) مع 7 لغات + تحسينات أداء (Skeleton/Lazy)                                                                                                                                                                      |
| Frontend | صفحات المتجر: Restaurants List + Restaurant Details/Menu (+ معاينة صور + إيماءات لمس) + Multi-Cart + CartSidebar (FAB + sidebar متعدد المطاعم) + Checkout                                                                                |
| Frontend | Notifications UI (MVP): قائمة + badge + Mark as read + realtime                                                                                                                                                                          |
| Frontend | إدارة صور اللاندنج (Hero + 4 صور Why Choose Us) عبر Cloudinary                                                                                                                                                                           |
| Frontend | صفحة About (Wolt-inspired) مع تقليل السكاشن + تحسينات UI/Contrast                                                                                                                                                                        |
| Frontend | إدارة صور صفحة About عبر الأدمن (Slots مستقلة عن اللاندنج)                                                                                                                                                                               |
| Frontend | أزرار الـ Cute Navbar تعمل كتبديل (Tabs) لعرض الصور والنصوص ديناميكياً                                                                                                                                                                   |
| Frontend | تحسين نظام الثيم: Dark افتراضي + منع `prefers-color-scheme` من تجاوز اختيار المستخدم + تعيين `data-theme` على عنصر `html`                                                                                                                |
| Frontend | توحيد ستايل المتجر (Restaurants list/details): إزالة gradients + ربط الألوان بـ tokens الثيم (accent cyan)                                                                                                                               |
| Frontend | Partner Places (Admin): عرض الطلبات + Actions (Approve/Needs info/Reject/Message/Delete) + تحديث لحظي                                                                                                                                    |
| Frontend | Admin Dashboard: بيانات حقيقية من API (counts + pending actions) + كارت “Providers missing bank details”                                                                                                                                 |
| Frontend | Partner Places (Admin): عرض بيانات البنك (IBAN/SWIFT) بشكل masked داخل Modal + Copy-to-clipboard                                                                                                                                         |
| Frontend | Restaurants Page Redesign: عنوان "Our Providers" + بحث فوري + كاروسيل تصنيفات (أسهم + سحب + تمرير تلقائي لا نهائي) + كاروسيل عروض ترويجية + بانر أفضل عرض في تفاصيل المطعم + شارات Open/Closed                                           |
| Frontend | Promotional Offers v2 (Admin): إنشاء/تعديل/حذف عروض بانر مرئية + عناوين ثنائية اللغة + شروط مرنة + ربط بمطاعم + صلاحية زمنية                                                                                                             |
| Frontend | تسجيل دخول تلقائي بعد التسجيل + كود اتصال دولي عند التسجيل + تحقق من عدد أرقام الهاتف حسب الدولة                                                                                                                                         |
| Frontend | إصلاح Error Toast: تجاهل 401/403 عالمياً + GlobalErrorHandler يتجاهل HttpErrorResponse + X-Skip-Error-Toast header                                                                                                                       |
| Frontend | Partner Portal (Provider): تسجيل دخول + منع تعارض الجلسات (log out first) + ترجمة الرسالة + شارة Provider في navbar                                                                                                                      |
| Frontend | Provider System Settings: صفحة إعدادات (General/Hours/Branches/Danger Zone) محمية بإعادة إدخال كلمة المرور + Unlock قصير عبر sessionStorage                                                                                              |
| Frontend | Provider Dashboard: إضافة إدارة الأصناف (Categories) وعناصر المنيو (Menu Items) (Add/Edit/Delete) مع رفع صورة العنصر + Availability + ربط Category + عرض السعر باليورو                                                                   |
| Frontend | Admin Promotions: إنشاء/حذف Promo codes + ربطها بمطاعم محددة + صلاحية (start/end) + إرسال إشعارات لمستخدمين محددين + عرض قائمة Active (غير منتهية)                                                                                       |
| Frontend | Store UX: جعل الصفحة الرئيسية للمستخدم تفتح `/restaurants` تلقائياً + عرض إجمالي/فاتورة الكارت بعملة EUR                                                                                                                                 |
| Frontend | Drivers (MVP): صفحة إدارة السائقين في الأدمن + Driver Login + Driver Dashboard (Online/Location + Assigned orders) + تحديث لحظي                                                                                                          |
| Frontend | Drivers (Updates): زر فصل/تفعيل السائق + إرسال رسائل للسائق + Inbox/Bell في Driver Dashboard + تحديث لحظي بدون refresh                                                                                                                   |
| Frontend | Startup Service Status: عند فتح الموقع يظهر Toast بحالة API/DB/Redis                                                                                                                                                                     |
| Backend  | Provider/Partner APIs: `GET /partner-places/me` + تحديثات `PATCH /partner-places/me/request` + رفع صورة المطعم + CRUD للأصناف/المنيو scoped على مطعم الـ Provider                                                                        |
| Backend  | Admin Dashboard API: `GET /api/v1/admin-staff/dashboard/overview` لإرجاع counts + pending (provider requests / support tickets / missing bank details)                                                                                   |
| Docs     | توثيق موجود (ملفات في الجذر + مجلد `docs/`) لكن يحتاج مراجعة/توحيد مع الواقع الحالي                                                                                                                                                      |

---

## 🧾 جرد المشروع (Inventory)

> هذا القسم يوثق ما هو موجود فعلاً في الريبو (Modules/Pages/Services) لتحديد الناقص/غير المستخدم بدقة.

### Backend (NestJS)

- Modules (مُستخدمة داخل `AppModule`):
  - `admin-staff`, `users`, `auth`, `payments`, `support-tickets`, `site-settings`, `restaurants`, `orders`, `promotions`, `notifications`, `partner-places`, `drivers`, `ratings` **(13 modules)**
- CommonModule (Global):
  - **26 خدمة مشتركة** (Security/Performance/Monitoring/Storage/Communication/Realtime/Payment/Database…) + 3 controllers
  - Redis cache اختياري عبر `CACHE_REDIS_ENABLED` (fallback إلى in-memory).

### Web Frontend (Angular)

- Pages/Areas (موجودة):
  - Landing, Restaurants, Cart/Checkout, User (Profile + Legal), Admin (dashboard + site settings + partner places), Provider portal.
- Driver pages: Driver Login + Driver Dashboard (MVP) مع realtime للأوردرات + online/location.

### Mobile (Flutter)

- هيكل Flutter موجود لكن **لا يوجد تطبيق فعلي** — فقط scaffold الافتراضي. المجلد `mfrontend/` لم يُبنى عليه أي ميزة بعد.

### تقدير نسب الإنجاز التفصيلية (Estimates)

> هذه نسب تقريبية مبنية على وجود الـ endpoints/الصفحات والـ flows الأساسية، وليست مبنية على تغطية اختبارات.

#### Backend Modules

| الوحدة          | نسبة الإنجاز | ملاحظات مختصرة                                                                                                                                    |
| --------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Auth            | 92%          | Login/Refresh/Logout/Sessions + **Google OAuth** + **forgot-password (3-step)**                                                                   |
| Users           | 90%          | CRUD + تحقق Email/Phone (OTP) + عناوين + وسائل دفع (Stripe) + **Moien Coins** + **add-coins (admin)**                                             |
| Restaurants     | 90%          | CRUD + categories + menu items + **addons (JSONB)** + geocode proxy                                                                               |
| Orders          | 92%          | Cart + Multi-Cart + Checkout + Tracking + **Wallet checkout/topup** + **cashback 1%** + **auto-apply offers** + **free delivery 30d** + email+SMS |
| Drivers         | 80%          | Admin drivers + Online/Location + deactivate/activate + force logout + رسائل + **driver applications** + lifecycle + realtime                     |
| Payments        | 85%          | Stripe + **PayPal** + webhook + **Moien Coins**؛ لا يوجد settlement flow                                                                          |
| Promotions      | 88%          | Admin CRUD + صلاحية + مطاعم + Validate + إشعارات + **promotional offers (auto-apply)**                                                            |
| Notifications   | 75%          | DB + Socket.IO + Delete/Clear + TTL cleanup؛ Push غير موجود                                                                                       |
| Partner Places  | 95%          | Workflow كامل + فروع + إدارة المنيو/**الإضافات** + **قبول/رفض طلبات** + **تحليلات المبيعات** + **طلبات تجريبية**                                  |
| Site Settings   | 88%          | صور landing/about/**coins** + CRUD إعدادات                                                                                                        |
| Support Tickets | 65%          | إنشاء/عرض/رد/حذف؛ يحتاج SLA/attachments                                                                                                           |
| Admin Staff     | 85%          | Login + CRUD + **11 صلاحية** + Dashboard + **Analytics** + **Provider Analytics** + XLSX export                                                   |

#### Web Areas

| الجزء             | نسبة الإنجاز | ملاحظات مختصرة                                                                                                                                                |
| ----------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Landing/About     | 90%          | UI قوي + i18n؛ بعض اللغات تحتوي سلاسل غير مترجمة بالكامل                                                                                                      |
| Restaurants/Store | 95%          | Browse + details/menu + بحث + carousels + عروض + cart sidebar (FAB + multi-restaurant) + preview modal + touch gestures (pinch/pan/double-tap) + theme tokens |
| Checkout          | 85%          | MVP checkout + promo + Stripe Payment Element + Moien Coins wallet + PayPal + cashback 1%                                                                     |
| User Profile      | 82%          | بيانات + عناوين + payment methods (Stripe) + كود اتصال دولي + تحقق أرقام + password reset + Google OAuth                                                      |
| Admin             | 90%          | إدارة صور + Partner Places + Promotions + Drivers + Dashboard + Provider Analyst + Coins Images + Add Coins                                                   |
| Provider Portal   | 93%          | Login + System Settings gate + Dashboard + إدارة المنيو/الأصناف + Addons + Accept/Reject + Sales Analytics                                                    |
| Driver            | 68%          | Driver Login + Dashboard (online/location + assigned orders) + inbox + accept/cancel/delivered + Leaflet map fix + routing (MVP)                              |

---

## 🚨 المشاكل الحرجة (Critical Issues)

> **⚠️ يجب معالجتها فوراً قبل الإطلاق**

### 1. 🔐 الأمان (Security)

| المشكلة                          | الخطورة         | الوصف                                                                                    |
| -------------------------------- | --------------- | ---------------------------------------------------------------------------------------- |
| **Rate Limiting (فعلي)**         | ✅ تم           | تفعيل ThrottlerGuard عالمياً + إمكانية ضبطه عبر env                                      |
| **تشفير كلمات المرور بـ bcrypt** | ✅ تم           | `passwordHash` يتم توليده/مقارنته في Users Service                                       |
| **CORS محكم**                    | ✅ تم           | origins قابلة للضبط عبر `CORS_ORIGINS` مع whitelist                                      |
| **Helmet للـ Headers**           | ✅ تم           | تفعيل `helmet()` افتراضياً (CSP off by default)                                          |
| **CSRF Protection**              | 🟡 اختياري      | Middleware (Double-submit cookie) عبر `CSRF_ENABLED=true` + لا يطبق مع Bearer auth       |
| **SQL Injection Protection**     | 🟡 مغطى جزئياً  | TypeORM parameterization + ValidationPipe (مع استمرار المراجعة عند استخدام QueryBuilder) |
| **Input Validation شامل**        | ✅ تم (أساسياً) | Global ValidationPipe (whitelist/forbid/transform) + DTOs موجودة لمعظم الـ modules       |
| **Email Role Uniqueness**        | ✅ تم           | لا يمكن استخدام نفس البريد كـ User و Provider (حماية على approve + provider login)       |

### 2. 🗄️ قاعدة البيانات (Database)

| المشكلة                 | الخطورة | الوصف                                                                                                              |
| ----------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| **Database Migrations** | ✅ تم   | إضافة TypeORM migrations + DataSource + scripts لتشغيلها بأمان                                                     |
| **Database Seeding**    | ✅ تم   | سكربت `db:seed` لإضافة بيانات تجريبية (users/restaurants/menu/promo)                                               |
| **Database Indexes**    | ✅ تم   | إضافة indexes أساسية عبر migration لتحسين الأداء على الجداول الأكثر استخداماً                                      |
| **Delete Policy**       | ✅ تم   | تحويل الحذف في أجزاء واسعة إلى **Hard Delete** عند طلب المستخدم، مع الحفاظ على سجلات تاريخية (مثل Orders/Payments) |

### 3. 🔑 المصادقة (Authentication)

| المشكلة                | الخطورة | الوصف                                                         |
| ---------------------- | ------- | ------------------------------------------------------------- |
| **Refresh Token**      | ✅ تم   | Refresh token opaque + rotation عبر sessions في DB            |
| **Token Blacklisting** | ✅ تم   | إبطال الجلسة (revoke session) يبطل access tokens المرتبطة بها |
| **Session Management** | ✅ تم   | جدول sessions + list/revoke endpoints لتتبع الجلسات النشطة    |
| **لا يوجد OAuth**      | ✅ تم   | **Google OAuth** موجود (`POST auth/user/google`)              |

---

## 🔧 الأشياء التي تحتاج إصلاح (Bugs & Fixes)

### Backend

| #   | المشكلة                                         | الملف/الموقع                             | الأولوية        |
| --- | ----------------------------------------------- | ---------------------------------------- | --------------- |
| 1   | الخدمات المشتركة كانت محسوبة placeholder        | `backend/src/common/services/*`          | ✅ تم           |
| 2   | `main.ts` يحتاج تفعيل Validation Pipe بشكل صحيح | `backend/src/main.ts`                    | ✅ تم           |
| 3   | Error handling غير موحد                         | جميع الـ controllers                     | ✅ تم           |
| 4   | لا يوجد logging فعلي للأخطاء                    | `backend/src/common/services/monitoring` | ✅ تم           |
| 5   | عدم وجود response interceptor موحد              | `backend/src/common/interceptors`        | ✅ تم (اختياري) |
| 6   | تحذير DevTools: `/favicon.ico` يرجع 404 (Noise) | `backend/src/main.ts`                    | ✅ تم           |
| 7   | `npm run start:dev` قد يفشل بـ `EADDRINUSE`     | تشغيل محلي (PORT=3000)                   | 🟡 متوسط        |

### Frontend (Angular)

| #   | المشكلة                                            | الملف/الموقع                                                                                    | الأولوية     |
| --- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ------------ |
| 1   | لا يوجد Error Handling موحد                        | `wfrontend/src/app/interceptors/error.interceptor.ts` + `utils/global-error-handler.ts`         | ✅ تم (شامل) |
| 2   | لا يوجد Loading State عام                          | `wfrontend/src/app/services/loading.ts` + `components/global-loading/*`                         | ✅ تم        |
| 3   | لا يوجد HTTP Interceptor موحد (Auth/Errors/Retry)  | `wfrontend/src/app/app.config.ts` + `wfrontend/src/app/interceptors/*`                          | ✅ تم        |
| 4   | بعض الصفحات لا تتأثر بتغيير الثيم بسبب ألوان ثابتة | صفحات User/Store                                                                                | ✅ تم        |
| 5   | لا يوجد PWA support                                | -                                                                                               | 🟡 منخفض     |
| 6   | لا يوجد SEO optimization                           | -                                                                                               | 🟡 منخفض     |
| 7   | تحذير DevTools: بعض حقول الإدخال بدون `id/name`    | `wfrontend/src/app/pages/landing/landing.v2.html`                                               | ✅ تم        |
| 8   | بعض الترجمات غير مكتملة (سلاسل EN داخل لغات أخرى)  | `wfrontend/src/app/services/language.ts`                                                        | 🟡 متوسط     |
| 9   | إضافة وسيلة دفع (Add Card)                         | `wfrontend/src/app/pages/user/profile/*` + `backend/src/modules/users/users-billing.service.ts` | ✅ تم        |
| 10  | اختبار واجهة الويب يفشل بسبب توقع وجود `h1`        | `wfrontend/src/app/app.spec.ts`                                                                 | ✅ تم        |

### Mobile (Flutter)

| #   | المشكلة                                             | الأولوية |
| --- | --------------------------------------------------- | -------- |
| 1   | **التطبيق فارغ تماماً** - فقط `main.dart` الافتراضي | 🔴 حرج   |

---

## 📈 الأشياء التي تحتاج تحسين (Improvements)

### 1. 🏗️ الهيكل والتنظيم (Architecture)

| #   | التحسين                       | الوصف                                                 | الأولوية |
| --- | ----------------------------- | ----------------------------------------------------- | -------- |
| 1   | **فصل DTOs**                  | ✅ تم جزئياً (Users module: Response DTOs في `dto/*`) | 🔴 عالي  |
| 2   | **Repository Pattern**        | ✅ تم جزئياً (UsersRepository كمثال)                  | 🟠 متوسط |
| 3   | **Event-Driven Architecture** | ✅ تم جزئياً (users.registered → welcome email)       | 🟠 متوسط |
| 4   | **Monorepo Tools**            | ✅ تم (npm workspaces + root scripts)                 | 🟡 منخفض |

### 2. ⚡ الأداء (Performance)

| #   | التحسين                         | الوصف                                                                       | الأولوية |
| --- | ------------------------------- | --------------------------------------------------------------------------- | -------- |
| 1   | **Redis Caching**               | ✅ تم (CacheModule + caching فعلي للمطاعم/القوائم/التصنيفات + invalidation) | 🔴 عالي  |
| 2   | **Database Connection Pooling** | ✅ تم (pg pool عبر TypeORM `extra` + env `DB_POOL_*`)                       | 🟠 متوسط |
| 3   | **API Response Compression**    | ✅ تم (express compression middleware)                                      | 🟠 متوسط |
| 4   | **Image Optimization**          | 🟡 جزئي (Cloudinary transforms في الويب + جاهز للتوسيع)                     | 🟡 منخفض |
| 5   | **CDN Integration**             | 🟡 جزئي (Cache-Control للـ uploads + جاهز لوضع CDN قدام /uploads)           | 🟡 منخفض |

### 3. 🧪 الاختبارات (Testing)

| #   | التحسين               | الوصف                       | الأولوية |
| --- | --------------------- | --------------------------- | -------- |
| 1   | **Unit Tests**        | تغطية 80% على الأقل         | 🔴 عالي  |
| 2   | **Integration Tests** | اختبارات للـ APIs           | 🔴 عالي  |
| 3   | **E2E Tests**         | اختبارات شاملة للسيناريوهات | 🟠 متوسط |
| 4   | **Load Testing**      | اختبارات الحمل              | 🟡 منخفض |

### 4. 📚 التوثيق (Documentation)

| #   | التحسين                | الوصف                   | الأولوية |
| --- | ---------------------- | ----------------------- | -------- |
| 1   | **Swagger/OpenAPI**    | توثيق تفاعلي للـ API    | 🔴 عالي  |
| 2   | **Postman Collection** | مجموعة requests جاهزة   | 🟠 متوسط |
| 3   | **Storybook**          | توثيق الـ UI components | 🟡 منخفض |

### 5. 🔧 DevOps

| #   | التحسين                   | الوصف                                                               | الأولوية |
| --- | ------------------------- | ------------------------------------------------------------------- | -------- |
| 1   | **Docker Compose**        | بيئة تطوير متكاملة                                                  | 🔴 عالي  |
| 2   | **CI/CD Pipeline**        | GitHub Actions للنشر الآلي                                          | 🔴 عالي  |
| 3   | **Environment Variables** | إدارة أفضل للمتغيرات                                                | 🟠 متوسط |
| 4   | **Health Checks**         | ✅ تم (Basic): `GET /api/v1/health/services` + إشعار عند فتح الموقع | 🟠 متوسط |
| 5   | **Kubernetes Manifests**  | للنشر على السحابة                                                   | 🟡 منخفض |

---

## 🔐 متغيرات البيئة (.env) وتشغيل المشروع

هذا القسم مُحدّث ليعكس المتغيرات **المستخدمة فعلاً في الكود** داخل الـ Backend، وما يلزم لتشغيل الموقع بكفاءة.

### أين تضع ملف .env؟

- المسار المفضل: `backend/.env`
- الـ backend يحاول تحميل `.env` تلقائياً من عدة مسارات (مع تفضيل `backend/.env`).

### الحد الأدنى لتشغيل الـ Backend محلياً (MVP)

> هذه المتغيرات تكفي لتشغيل الـ API + تسجيل دخول الأدمن + تشغيل migrations.

```env
NODE_ENV=development
PORT=3000
API_PREFIX=api/v1

DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=moien_delivery
DATABASE_USER=postgres
DATABASE_PASSWORD=your_password

# ضروري لتسجيل الدخول بشكل آمن (حتى لو توجد defaults، لا تعتمد عليها)
JWT_SECRET=change_me_min_32_chars
JWT_REFRESH_SECRET=change_me_min_32_chars

# مهم جداً: migrations سترفض إنشاء super admin بدونهم
DEFAULT_SUPER_ADMIN_EMAIL=admin@moien.com
DEFAULT_SUPER_ADMIN_PASSWORD=change_me

# لينكات الإيميلات/الروابط (fallback إذا لم تضع APP_PUBLIC_URL/PUBLIC_URL)
FRONTEND_URL=http://localhost:4200
```

### تشغيل قاعدة البيانات والمهاجرات

- في التطوير عادةً شغّل: `npm run db:migrate:run`
- ثم (اختياري) بيانات تجريبية: `npm run db:seed`

### تحسينات موصى بها للتطوير/الإنتاج

```env
# Public URL used in emails / deep links (if unset, falls back to FRONTEND_URL)
APP_PUBLIC_URL=
PUBLIC_URL=

# CORS
CORS_ORIGINS=http://localhost:4200

# Rate limiting (افتراضياً موجود؛ عدّل حسب الحاجة)
THROTTLE_TTL_SEC=60
THROTTLE_LIMIT=120

# Compression
COMPRESSION_ENABLED=true
COMPRESSION_THRESHOLD_BYTES=1024

# Reverse proxy / response shaping (optional)
TRUST_PROXY=false
RESPONSE_ENVELOPE=false

# Uploads caching
UPLOADS_CACHE_MAXAGE_SECONDS=3600

# TypeORM / DB pool
DB_POOL_MAX=20
DB_POOL_IDLE_TIMEOUT_MS=30000
DB_POOL_CONNECTION_TIMEOUT_MS=2000

# Monitoring (optional; keep false in production by default)
MONITORING_LOGS_PUBLIC=false

# Admin-only “master code” for sensitive analytics endpoint
ADMIN_MASTER_CODE=1234

# CSRF (optional; only enable if you need cookie-based protection)
CSRF_ENABLED=false
CSRF_COOKIE_NAME=XSRF-TOKEN
```

### ميزات اختيارية “للكفاءة” (تشتغل فقط لو ضبطت مفاتيحها)

#### Cloudinary (رفع الصور للأدمن/المستخدمين/المطاعم)

> بدونها: أي endpoint رفع صورة سيُرجع خطأ “Cloudinary is not configured”.

```env
# إما تضبط CLOUDINARY_URL
# CLOUDINARY_URL=cloudinary://<api_key>:<api_secret>@<cloud_name>

# أو تضبط الثلاثي التالي:
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=

# Optional
CLOUDINARY_FOLDER=moien/admin-staff
CLOUDINARY_USERS_FOLDER=moien/users
```

#### Stripe (الدفع)

```env
STRIPE_SECRET_KEY=
STRIPE_PUBLISHABLE_KEY=
STRIPE_WEBHOOK_SECRET=
```

#### Email (SMTP) — إرسال رسائل (best-effort)

> إذا لم تضبط `MAIL_*` سيتم “محاكاة الإرسال” في اللوج بدل SMTP.

```env
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_SECURE=false
MAIL_USER=
MAIL_PASS=
MAIL_FROM=
MAIL_REPLY_TO=
```

#### SMS / WhatsApp (Twilio)

```env
SMS_ENABLED=true
SMS_PROVIDER=twilio
SMS_SENDER=

TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
TWILIO_PHONE_NUMBER=

# Optional advanced
TWILIO_MESSAGING_SERVICE_SID=
TWILIO_SMS_FROM=

TWILIO_WHATSAPP_ENABLED=true
TWILIO_WHATSAPP_FROM=
TWILIO_DEFAULT_COUNTRY_CALLING_CODE=20
```

#### Redis Cache (اختياري)

> إذا `CACHE_REDIS_ENABLED=true` ولم تكن إعدادات Redis صحيحة سيحصل degrade (fallback in-memory) حسب المسار.

```env
CACHE_TTL_MS=60000
CACHE_MAX_ITEMS=10000

CACHE_REDIS_ENABLED=false
REDIS_URL=
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_TLS=false
```

### ملاحظة مهمة عن Web Frontend (.env)

- الويب (Angular) لا يستخدم `.env` وقت التشغيل. تحديد عنوان الـ API يتم من ملفات environments:
  - `wfrontend/src/environments/environment.development.ts`
  - `wfrontend/src/environments/environment.production.ts`

### مفاتيح موجودة في backend/.env.example لكنها غير مستخدمة حالياً

- مفاتيح مثل `GOOGLE_MAPS_API_KEY`, `FIREBASE_*`, `AWS_*`, `MOBILE_DEEP_LINK` موجودة في المثال، لكن لا يوجد لها استخدام فعلي واضح في الكود الحالي.

---

## 🧩 أجزاء موجودة لكن غير مستخدمة/غير مكتملة

| الجزء                         | أين موجود                                                         | الحالة الحالية                                     | ملاحظات عملية                                                                                                                         |
| ----------------------------- | ----------------------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Search/Analytics/Queue/Backup | `backend/src/common/services/*` + صفحات status في `AppController` | 🟡 موجودة كـ services لكن غير مكشوفة كـ APIs فعلية | يوجد صفحات status فقط، ولا توجد Controllers عامة لـ search/analytics/queue/backup حتى الآن.                                           |
| SMS/WhatsApp (Twilio)         | `backend/src/common/services/communication/*`                     | 🟡 تكامل موجود ويعتمد على إعداد الحساب             | SMS/WhatsApp يعملان عبر Twilio؛ WhatsApp يتطلب sender مُفعّل (أو Sandbox). SMS قد يتطلب Geo Permissions لكل دولة.                     |
| Driver Web Pages              | `wfrontend/src/app/pages/driver/*`                                | 🟡 MVP موجود                                       | يوجد Driver login + Dashboard (online/location + inbox + accept/cancel/delivered). ينقص UX متقدم (earnings/settlement/advanced maps). |
| Mobile App                    | `mfrontend/`                                                      | 🔴 Skeleton                                        | مشروع Flutter موجود (platforms + build) لكن بدون features فعلية.                                                                      |
| Docs Consistency              | `CATALOG.md` + `ARCHITECTURE.md`                                  | 🟡 يحتاج مراجعة                                    | تم تحديثهما ليعكسا الواقع الحالي (v0.0.13) لكن يحتاجان مراجعة دورية مع كل إصدار جديد.                                                 |

## 📦 الميزات الناقصة (Missing Features)

### الوحدات الأساسية (Core Modules)

| #   | الوحدة                | الوصف                        | الأولوية | الحالة                                                                            |
| --- | --------------------- | ---------------------------- | -------- | --------------------------------------------------------------------------------- |
| 1   | 🍴 **Restaurants**    | إدارة المطاعم والقوائم       | 🔴 حرج   | ✅ تم (Backend + Web MVP)                                                         |
| 2   | 🛒 **Orders**         | نظام الطلبات والسلة          | 🔴 حرج   | ✅ تم (Cart + Checkout MVP)                                                       |
| 3   | 🚗 **Drivers**        | إدارة السائقين والتوصيل      | 🔴 حرج   | 🟡 تم (Backend + Web MVP) — ما زالت تنقص خرائط/تتبع متقدم + أرباح/تسوية           |
| 4   | 📍 **Delivery Zones** | مناطق التوصيل والتغطية       | 🔴 حرج   | ❌ لم يبدأ                                                                        |
| 5   | 🏷️ **Categories**     | تصنيفات المطاعم والأطباق     | 🟠 عالي  | 🟡 جزئي (Restaurant Categories)                                                   |
| 6   | ⭐ **Reviews**        | نظام التقييمات والمراجعات    | 🟠 عالي  | 🟡 جزئي (Backend Ratings module موجود؛ ينقص UI)                                   |
| 7   | 🎁 **Promotions**     | الكوبونات والعروض            | 🟠 عالي  | ✅ تم (Admin CRUD + صلاحية start/end + ربط مطاعم + validate/apply + notify users) |
| 8   | 🔔 **Notifications**  | الإشعارات (Push, Email, SMS) | 🟠 عالي  | 🟡 DB + Socket.IO ✅ / Push ❌ / SMS/WhatsApp (Twilio) ✅\*                       |

\* ملاحظة: التكامل موجود، لكن يحتاج إعداد Twilio (sender/geo permissions) حسب الدولة.

### الميزات الإضافية (Additional Features)

| #   | الميزة                     | الوصف                                                                  | الأولوية |
| --- | -------------------------- | ---------------------------------------------------------------------- | -------- |
| 1   | 🔍 **Search & Filters**    | بحث متقدم وفلاتر                                                       | 🟠 عالي  |
| 2   | 🗺️ **Real-time Tracking**  | تتبع الطلب على الخريطة (جزئي: realtime updates + live driver location) | 🟠 عالي  |
| 3   | 💬 **Chat System**         | محادثة بين المستخدم والسائق                                            | 🟠 عالي  |
| 4   | 📊 **Analytics Dashboard** | لوحة إحصائيات للمطاعم                                                  | 🟠 عالي  |
| 5   | 🔄 **Order Scheduling**    | جدولة الطلبات مسبقاً                                                   | 🟡 متوسط |
| 6   | 👥 **Group Orders**        | طلبات جماعية                                                           | 🟡 متوسط |
| 7   | 🎯 **Recommendations**     | توصيات ذكية                                                            | 🟡 متوسط |
| 8   | 💰 **Wallet System**       | ✅ **تم** — Moien Coins (شحن + دفع + cashback 1%)                      | ✅ تم    |
| 9   | 🏆 **Loyalty Program**     | برنامج الولاء والنقاط                                                  | 🟡 متوسط |
| 10  | 📱 **Deep Linking**        | روابط مباشرة للتطبيق                                                   | 🟡 متوسط |
| 11  | 🔑 **Password Reset**      | ✅ **تم** — 3 خطوات (request → verify → reset) عبر email أو SMS        | ✅ تم    |
| 12  | 🔐 **Google OAuth**        | ✅ **تم** — `POST auth/user/google`                                    | ✅ تم    |

### واجهة الويب (Web Frontend) — الحالة الحالية

| #   | الصفحة/الميزة             | الوصف               | الأولوية |
| --- | ------------------------- | ------------------- | -------- |
| 1   | 🏠 **Landing Page**       | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 2   | 🍴 **Restaurant List**    | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 3   | 📋 **Restaurant Menu**    | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 4   | 🛒 **Shopping Cart**      | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 5   | 💳 **Checkout**           | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 6   | 📍 **Order Tracking**     | ✅ تم تنفيذها (MVP) | 🔴 حرج   |
| 7   | 📜 **Order History**      | ✅ تم تنفيذها (MVP) | 🟠 عالي  |
| 8   | 👤 **User Profile**       | ✅ تم تنفيذها (MVP) | 🟠 عالي  |
| 9   | 🏪 **Provider Dashboard** | ✅ تم تنفيذها (MVP) | 🟠 عالي  |
| 10  | 🚗 **Driver Dashboard**   | 🟡 تم تنفيذها (MVP) | 🟠 عالي  |

> ملاحظة: تم تنفيذ Flow أساسي للمتجر (Restaurants → Menu → Cart → Checkout)، وباقي صفحات المتجر ما زالت ضمن الميزات الناقصة.

### تطبيق الموبايل (Mobile App)

| #   | الشاشة           | الوصف                | الأولوية |
| --- | ---------------- | -------------------- | -------- |
| 1   | **جميع الشاشات** | التطبيق لم يبدأ بعد! | 🔴 حرج   |

---

## 🌟 مميزات تميزنا عن المنافسين (Competitive Advantages)

> **💡 أفكار مبتكرة لتمييز التطبيق عن المنافسين**
>
> ملاحظة: هذا القسم أفكار/اقتراحات غير منفذة حالياً، وعمود “التأثير” تقديري وليس أرقاماً مؤكدة.

### 1. 🤖 الذكاء الاصطناعي (AI Features)

| الميزة                   | الوصف                                      | التأثير                    |
| ------------------------ | ------------------------------------------ | -------------------------- |
| **🎯 توصيات ذكية**       | AI يتعلم تفضيلات المستخدم ويقترح أطباق     | ⬆️ زيادة المبيعات (تقديري) |
| **⏱️ تنبؤ وقت التوصيل**  | AI يحسب الوقت بناءً على حركة المرور والطقس | ⬆️ رضا العملاء             |
| **📸 التعرف على الطعام** | المستخدم يصور طبق والـ AI يجده في التطبيق  | 🆕 ميزة فريدة              |
| **💬 Chatbot ذكي**       | دعم فني آلي بالعربية واللهجات المحلية      | ⬇️ تقليل تكلفة الدعم       |
| **🔮 تنبؤ الطلبات**      | AI يتنبأ بالطلبات للمطاعم للتحضير المسبق   | ⬆️ سرعة التوصيل            |

### 2. 🎮 التحفيز والألعاب (Gamification)

| الميزة                    | الوصف                              | التأثير                  |
| ------------------------- | ---------------------------------- | ------------------------ |
| **🏆 نظام المستويات**     | المستخدم يصعد مستويات حسب الطلبات  | ⬆️ زيادة الولاء (تقديري) |
| **🎯 تحديات يومية**       | "اطلب من 3 مطاعم جديدة واربح خصم"  | ⬆️ تنويع الطلبات         |
| **🎰 عجلة الحظ**          | فرصة للفوز بخصومات بعد كل طلب      | 🎉 تجربة ممتعة           |
| **🏅 شارات الإنجاز**      | شارات خاصة (أول طلب، 100 طلب، إلخ) | 📢 مشاركة اجتماعية       |
| **👥 تحديات مع الأصدقاء** | من يطلب أكثر هذا الشهر             | 📈 زيادة الاستخدام       |

### 3. 🌍 ميزات اجتماعية (Social Features)

| الميزة               | الوصف                                      | التأثير            |
| -------------------- | ------------------------------------------ | ------------------ |
| **📍 طلبات جماعية**  | مجموعة أصدقاء يطلبون معاً ويقسمون الفاتورة | 🆕 ميزة فريدة      |
| **📸 مشاركة الطعام** | المستخدم يشارك صور طلباته                  | 📢 تسويق مجاني     |
| **🎁 هدية لصديق**    | إرسال طلب كهدية مفاجئة                     | 💝 engagement عالي |
| **📜 قوائم مشتركة**  | "أطباقنا المفضلة" مع العائلة               | 👨‍👩‍👧‍👦 استخدام عائلي   |
| **🔗 مشاركة الموقع** | "أين أطلب؟" - أصدقاء يساعدون في الاختيار   | 💬 تفاعل اجتماعي   |

### 4. 🌱 الاستدامة والصحة (Sustainability & Health)

| الميزة                   | الوصف                                 | التأثير                |
| ------------------------ | ------------------------------------- | ---------------------- |
| **🌿 Carbon Footprint**  | حساب البصمة الكربونية لكل طلب         | 🌍 جذب العملاء الواعين |
| **♻️ تغليف صديق للبيئة** | خيار للمطاعم التي تستخدم تغليف مستدام | 🏷️ تمييز المطاعم       |
| **🥗 معلومات غذائية**    | سعرات حرارية ومكونات لكل طبق          | 🏥 جذب الواعين صحياً   |
| **⚠️ تنبيهات الحساسية**  | تنبيه تلقائي للمواد المسببة للحساسية  | 🛡️ سلامة العملاء       |
| **🚴 توصيل بالدراجات**   | خيار توصيل صديق للبيئة                | 🌿 صورة إيجابية        |

### 5. 💼 ميزات للأعمال (B2B Features)

| الميزة                  | الوصف                                 | التأثير           |
| ----------------------- | ------------------------------------- | ----------------- |
| **🏢 حسابات الشركات**   | إدارة طلبات الموظفين وميزانيات الغداء | 💰 عقود كبيرة     |
| **📊 تقارير للشركات**   | تقارير مفصلة عن الإنفاق               | 📈 قيمة مضافة     |
| **🎫 كوبونات الموظفين** | كود خاص لكل موظف                      | 🎯 تتبع الاستخدام |
| **📅 طلبات مجدولة**     | طلب غداء الشركة يومياً                | 🔄 إيرادات متكررة |
| **🍽️ Catering**         | طلبات كبيرة للمناسبات                 | 💵 أرباح عالية    |

### 6. 🔒 الخصوصية والأمان (Privacy & Security)

| الميزة                  | الوصف                        | التأثير         |
| ----------------------- | ---------------------------- | --------------- |
| **🕵️ وضع التخفي**       | إخفاء اسم المستخدم من السائق | 🛡️ أمان للنساء  |
| **📍 نقاط التقاء آمنة** | اقتراح أماكن عامة للاستلام   | 🛡️ أمان إضافي   |
| **🚨 زر الطوارئ**       | تنبيه فوري مع الموقع         | 🆘 سلامة        |
| **🔐 التحقق الثنائي**   | 2FA لحماية الحساب            | 🔒 ثقة المستخدم |
| **📊 تقرير البيانات**   | المستخدم يعرف ما نعرفه عنه   | ✅ شفافية       |

### 7. 🌐 ميزات محلية (Local Features)

| الميزة                     | الوصف                                    | التأثير            |
| -------------------------- | ---------------------------------------- | ------------------ |
| **🗣️ دعم اللهجات المحلية** | التطبيق يفهم اللهجة المصرية/الخليجية/إلخ | 🎯 سهولة الاستخدام |
| **📿 وجبات رمضان**         | فئة خاصة للإفطار والسحور                 | 🌙 موسم عالي       |
| **🕌 إشعارات الصلاة**      | إيقاف مؤقت للإشعارات وقت الصلاة          | 🙏 احترام          |
| **💵 الدفع النقدي**        | دعم الدفع عند الاستلام                   | 💰 ضروري محلياً    |
| **📞 رقم هاتف محلي**       | دعم فني بالهاتف المحلي                   | ☎️ ثقة أكبر        |

### 8. 🚀 ميزات تقنية متقدمة (Advanced Tech)

| الميزة                  | الوصف                              | التأثير         |
| ----------------------- | ---------------------------------- | --------------- |
| **📴 Offline Mode**     | تصفح القوائم بدون إنترنت           | 📶 تجربة أفضل   |
| **🔊 Voice Ordering**   | الطلب بالصوت                       | ♿ سهولة الوصول |
| **⌚ Apple/Wear Watch** | تطبيق للساعات الذكية               | ⚡ سرعة         |
| **🏠 Smart Home**       | "Ok Google, اطلب البيتزا المعتادة" | 🏠 راحة         |
| **🔗 API مفتوح**        | للشركات التي تريد التكامل          | 🤝 شراكات       |

---

## 🗓️ خريطة الطريق المقترحة

> ملاحظة: هذه خريطة طريق “مقترحة” وليست تقريراً عن حالة التنفيذ الحالية.

### المرحلة 1: الأساسيات (شهر 1-2)

```
┌─────────────────────────────────────────────────────────────────┐
│                     المرحلة 1: الأساسيات                        │
├─────────────────────────────────────────────────────────────────┤
│ ⬜ Week 1-2: إصلاح المشاكل الحرجة                              │
│    ├── تفعيل الأمان (Rate Limiting, CORS, Helmet)              │
│    ├── إضافة Database Migrations                               │
│    ├── تطبيق Refresh Tokens                                    │
│    └── إضافة Swagger Documentation                             │
│                                                                  │
│ ⬜ Week 3-4: الوحدات الأساسية                                  │
│    ├── إنشاء Restaurants Module                                │
│    ├── إنشاء Categories Module                                 │
│    ├── إنشاء Menu Items Module                                 │
│    └── إضافة Unit Tests                                        │
│                                                                  │
│ ⬜ Week 5-6: نظام الطلبات                                      │
│    ├── إنشاء Orders Module                                     │
│    ├── إنشاء Cart System                                       │
│    ├── تكامل Stripe للدفع                                      │
│    └── إضافة Order Status Tracking                             │
│                                                                  │
│ ⬜ Week 7-8: واجهة الويب                                       │
│    ├── Landing Page                                             │
│    ├── Restaurant Listing                                       │
│    ├── Menu & Cart                                              │
│    └── Checkout Flow                                            │
└─────────────────────────────────────────────────────────────────┘
```

### المرحلة 2: التوسع (شهر 3-4)

```
┌─────────────────────────────────────────────────────────────────┐
│                     المرحلة 2: التوسع                           │
├─────────────────────────────────────────────────────────────────┤
│ 🚗 Drivers Module                                                │
│    ├── تسجيل السائقين والتحقق                                  │
│    ├── نظام تعيين الطلبات                                      │
│    ├── تتبع الموقع Real-time                                   │
│    └── نظام الأرباح والتسوية                                   │
│                                                                  │
│ 📱 Mobile App (Flutter)                                          │
│    ├── تطبيق العملاء                                           │
│    ├── تطبيق السائقين                                          │
│    └── Push Notifications                                        │
│                                                                  │
│ 🔔 Notifications System                                          │
│    ├── Push Notifications (Firebase)                             │
│    ├── Email Notifications                                       │
│    └── SMS Notifications                                         │
│                                                                  │
│ ⭐ Reviews & Ratings                                             │
│    ├── تقييم المطاعم والأطباق                                  │
│    ├── تقييم السائقين                                          │
│    └── نظام المراجعات                                          │
└─────────────────────────────────────────────────────────────────┘
```

### المرحلة 3: التميز (شهر 5-6)

```
┌─────────────────────────────────────────────────────────────────┐
│                     المرحلة 3: التميز                           │
├─────────────────────────────────────────────────────────────────┤
│ 🤖 AI Features                                                   │
│    ├── توصيات ذكية                                             │
│    ├── تنبؤ وقت التوصيل                                        │
│    └── Chatbot                                                   │
│                                                                  │
│ 🎮 Gamification                                                  │
│    ├── نظام المستويات والنقاط                                  │
│    ├── الشارات والإنجازات                                      │
│    └── برنامج الولاء                                           │
│                                                                  │
│ 💼 B2B Features                                                  │
│    ├── حسابات الشركات                                          │
│    ├── Catering                                                  │
│    └── تقارير الإنفاق                                          │
│                                                                  │
│ 🌍 Social Features                                               │
│    ├── طلبات جماعية                                            │
│    ├── مشاركة الطعام                                           │
│    └── هدايا للأصدقاء                                          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 ملخص الأولويات

| الفئة           | العدد | الأولوية العالية | الأولوية المتوسطة | الأولوية المنخفضة |
| --------------- | ----- | ---------------- | ----------------- | ----------------- |
| 🚨 مشاكل حرجة   | 14    | 10               | 4                 | 0                 |
| 🔧 تحتاج إصلاح  | 10    | 3                | 4                 | 3                 |
| 📈 تحتاج تحسين  | 17    | 6                | 6                 | 5                 |
| 📦 ميزات ناقصة  | 28    | 12               | 10                | 6                 |
| 🌟 ميزات تميزنا | 40+   | -                | -                 | -                 |

---

## 🆕 آخر التحديثات (v0.0.14)

### Backend

| الميزة                          | الوصف                                                                                                                    |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Site Settings Module**        | وحدة جديدة لإدارة إعدادات الموقع                                                                                         |
| **Hero Image API**              | API لرفع وحذف صورة الهيرو سيكشن مع تخزين في Cloudinary                                                                   |
| **Landing Images API**          | API لإدارة صور اللاندنج (Hero + 4 صور Why Choose Us)                                                                     |
| **About Images API**            | API لإدارة صور صفحة About (5 Slots مستقلة)                                                                               |
| **Site Settings Entity**        | جدول جديد لتخزين إعدادات الموقع (key-value)                                                                              |
| **WebSocket Updates**           | أحداث Socket.IO لتحديث صور اللاندنج لحظياً                                                                               |
| **Restaurants API**             | CRUD للمطاعم + تصنيفات المطعم + عناصر القائمة                                                                            |
| **Orders/Cart API**             | Cart كـ Order + عمليات الإضافة/التعديل + Checkout                                                                        |
| **Orders (My Orders) API**      | Endpoints آمنة للمستخدم الحالي: `/orders/me` + `/orders/me/:id` + `/orders/me/:id/tracking`                              |
| **Promotions API**              | Endpoint للتحقق من الكوبونات + Tracking للاستخدام                                                                        |
| **Notifications API**           | تخزين إشعارات + realtime عبر Socket.IO                                                                                   |
| **Partner Places Module**       | Workflow كامل: طلب إضافة مطعم + قائمة الأدمن + approve/needs info/reject + إنشاء حساب Provider + إرسال Emails            |
| **Partner Places WebSocket**    | Namespace `/partner-places` + حدث `partner-places.changed` لتحديث صفحة الأدمن لحظياً                                     |
| **Delete Provider Master Code** | تأكيد حذف Provider/Request من الأدمن عبر Master Code + 3 محاولات خاطئة → حظر مؤقت مع countdown (عبر `/__access/*`)       |
| **Enterprise Email Theme**      | توحيد تصميم الإيميلات بشكل احترافي متوافق مع ألوان الأدمن داش                                                            |
| **Email Role Uniqueness**       | منع نفس الإيميل يكون User و Provider (حماية على approve + provider login)                                                |
| **Security Hardening**          | Rate limiting + CORS whitelist + Helmet + Validation + CSRF (اختياري)                                                    |
| **DB Migrations**               | TypeORM migrations + DataSource + scripts (`db:migrate:*`)                                                               |
| **DB Seeding**                  | Seed script (`db:seed`) لإنشاء بيانات تطوير جاهزة                                                                        |
| **DB Indexes**                  | Indexes أساسية على restaurants/menu/orders/notifications/...                                                             |
| **Hard Delete Migration**       | تحويل حذف كيانات/لوحات معينة إلى حذف نهائي (Hard Delete) حسب متطلبات المنتج، مع استثناءات لحفظ التاريخ (Orders/Payments) |
| **Common Platform Infra**       | CommonModule عالمي لتجميع وتصدير الـ shared services + DI جاهز                                                           |
| **Unified Error Handling**      | Global exception filter مع JSON موحد للأخطاء + requestId                                                                 |
| **Exception Log Noise**         | تقليل الضوضاء: 401/403/404 تُسجل كـ info بدل warn + إضافة userId/message للـ metadata                                    |
| **HTTP Logging**                | Global interceptor لتسجيل الطلبات والمدة + تجميع logs في LoggerService                                                   |
| **Monitoring Logs API**         | `/api/v1/monitoring/logs/*` (مقفولة في production إلا إذا `MONITORING_LOGS_PUBLIC=true`)                                 |
| **Response Envelope**           | Interceptor اختياري لتوحيد الردود `{ ok: true, data }` عبر `RESPONSE_ENVELOPE=true`                                      |
| **Auth Smoke Test**             | تم اختبار login → me → sessions → refresh → logout → refresh fails (401)                                                 |
| **Users DTO Split**             | نقل Response DTOs إلى `backend/src/modules/users/dto/*` (PublicUser/PublicSummary/...)                                   |
| **Users Repository**            | إضافة `UsersRepository` لفصل data-access عن UsersService                                                                 |
| **User Domain Events**          | حدث `users.registered` + listener لإرسال welcome email (best-effort)                                                     |
| **Root Monorepo Scripts**       | إضافة root `package.json` مع npm workspaces (backend + wfrontend)                                                        |
| **Redis-backed Cache**          | تفعيل Nest CacheModule مع Redis (اختياري) + fallback in-memory                                                           |
| **Hot Endpoints Caching**       | caching فعلي لـ Restaurants/Categories/MenuItems مع namespace version invalidation                                       |
| **HTTP Compression**            | تفعيل `compression` middleware لضغط responses (gzip/deflate)                                                             |
| **DB Pool Tuning**              | إضافة env `DB_POOL_*` وتمريرها لـ pg pool عبر TypeORM                                                                    |
| **Performance APIs**            | إضافة `/api/v1/performance/*` لعرض stats (cache/optimizer/response metrics/...)                                          |
| **Uploads Cache Headers**       | إضافة Cache-Control/ETag لـ `/uploads` مع max-age قابل للضبط                                                             |
| **Root Homepage (UX)**          | تحسين صفحة `/` لتكون أكثر احترافية + إضافة Access Modal gate                                                             |
| **Access Gate Endpoints**       | إضافة endpoints خارج الـ prefix: `GET /__access/status`, `POST /__access/check`, `POST /__access/devtools`               |
| **Access Refresh Policy**       | كل refresh يُطلب إدخال الكود مرة أخرى (بدون حفظ unlock في المتصفح)                                                       |
| **Access Lockout**              | 3 محاولات خاطئة → حظر 5 دقائق مع countdown فعلي                                                                          |
| **DevTools Suspension**         | رصد فتح DevTools (best-effort) → حظر 10 دقائق برسالة إنجليزية + countdown                                                |
| **Unusual Activity Monitor**    | مراقبة نشاط غير طبيعي (rate window) → حظر ساعة برسالة إنجليزية + countdown                                               |
| **CORS Same-Origin Fix**        | السماح لصفحة `/` بعمل fetch لـ `__access/*` عبر تضمين `http://localhost:${PORT}` ضمن whitelist                           |
| **Provider Me API**             | إضافة `GET /api/v1/partner-places/me` لقراءة بيانات الـ Provider الحالية مع request/branches                             |
| **Provider Request Update**     | إضافة `PATCH /api/v1/partner-places/me/request` لحفظ بيانات editable (ومنها opening hours)                               |
| **Provider Image Upload**       | رفع/تغيير صورة المطعم للـ Provider (Cloudinary) مع انعكاسها على الداشبورد                                                |
| **Provider Menu Management**    | CRUD للأصناف/المنيو scoped على مطعم الـ Provider + رفع صورة عنصر المنيو                                                  |

### تحديثات إضافية بعد v0.0.13 (حتى 18 فبراير 2026)

#### Backend

| التحديث                           | الوصف                                                                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Phone OTP Channel Routing**     | إرسال كود تحقق الهاتف عبر **SMS** أو **WhatsApp** حسب اختيار المستخدم (`channel=sms` أو `channel=whatsapp`) مع قوالب رسائل احترافية.  |
| **Twilio SMS/WhatsApp Hardening** | تحسين قراءة متغيرات البيئة (منع مشاكل cwd) + تحسين رسائل الخطأ (خصوصاً Geo Permissions و WhatsApp sender غير مُفعّل).                 |
| **Sender Branding Options**       | دعم استخدام Twilio **Messaging Service SID** و/أو Sender ID (حسب البلد/المشغل) لإظهار اسم مرسل مناسب بدل رقم عند الإمكان.             |
| **Auth/Register Robustness**      | معالجة تعارضات التسجيل/الدخول الناتجة عن بيانات محذوفة سابقاً + تحويل تعارض الـ unique إلى 409 بشكل واضح.                             |
| **API Prefix From ENV**           | جعل `API_PREFIX` من `.env` هو المصدر الفعلي للـ global prefix بدل hardcode.                                                           |
| **Stripe Webhook Receiver**       | إضافة Webhook endpoint للتحقق من `stripe-signature` باستخدام raw body + تسجيل event id/type بعد التحقق.                               |
| **Promotions Delete (Admin)**     | إضافة `DELETE /api/v1/promotions/:id` (Soft delete) + تنظيف روابط المطاعم المرتبطة بالعرض.                                            |
| **Orders: Address Gate**          | منع إتمام الطلب/الدفع فقط إذا المستخدم لا يمتلك أي عنوان (مع توجيه المستخدم لصفحة العناوين في الويب).                                 |
| **Tracking: Delivery Address**    | إرجاع `deliveryAddress` في tracking endpoint + عرض عنوان التسليم في صفحة Track بجانب عنوان المطعم.                                    |
| **Currency Normalization (EUR)**  | توحيد عرض العملة إلى EUR للطلبات القديمة التي كانت تُعرض EGP (وكذلك Stripe Checkout Session currency).                                |
| **Backend Unit Test Fix**         | إصلاح فشل اختبار `AppController` بإضافة mock لـ `ServerAccessService` داخل test module؛ الاختبارات أصبحت PASS.                        |
| **Drivers System (MVP)**          | إضافة Drivers module + Driver role/session + Admin drivers management + realtime + مشاركة موقع/online + best-effort nearest dispatch. |

### تحديثات إضافية (حتى 20 فبراير 2026)

#### Backend

| التحديث                              | الوصف                                                                                                              |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| **Driver Order Lifecycle**           | دعم accept/cancel/delivered + حفظ `driverAcceptedAt/driverCanceledAt/driverCancelReason` + migration جديدة.        |
| **Admin → Driver Messaging**         | إرسال رسائل للسائق (Socket.IO) + inbox persistent (Redis-backed cache best-effort) + TTL 3 أيام + clear/delete.    |
| **Admin Deactivate/Activate Driver** | تفعيل/فصل السائق + منع فصل السائق وهو Online + force logout لو تم فصل السائق.                                      |
| **User Orders Realtime Namespace**   | Namespace `/user-orders` بـ JWT user؛ إرسال orders snapshot + tracking payload كاملة (restaurant/delivery/driver). |
| **Service Health Endpoint**          | `GET /api/v1/health/services` لفحص API/DB/Redis مع latency وسبب الفشل.                                             |
| **Redis Production Hardening**       | دعم `REDIS_URL` + اختبار اتصال Redis + shim للتوافق مع `cache-manager` (delete/del).                               |

#### Web Frontend

| التحديث                           | الوصف                                                                   |
| --------------------------------- | ----------------------------------------------------------------------- |
| **Admin Drivers Actions**         | أزرار فصل/تفعيل + إرسال رسالة للسائق + تحديث لحظي للجدول.               |
| **Driver Inbox/Bell**             | أيقونة رسائل/Inbox في Driver Dashboard + unread counter + clear/delete. |
| **Driver Workflow Buttons**       | أزرار accept/cancel/delivered للأوردرات + عرض cancel reason/time.       |
| **Startup Service Status Toasts** | عند فتح الموقع يظهر Toast بحالة API/DB/Redis (OK/WARN/ERROR).           |

**Stripe webhook endpoint (Production):**

- `POST https://<DOMAIN>/{API_PREFIX}/payments/stripe/webhook`
- مثال شائع: `POST https://<DOMAIN>/api/v1/payments/stripe/webhook`

#### Web Frontend

| التحديث                  | الوصف                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Phone Verify UX**      | عند تحقق رقم الهاتف يظهر خيارين: SMS أو WhatsApp، والواجهات/الـ toasts تم توحيدها لتكون channel-agnostic.                                                     |
| **Add Card UX (Stripe)** | بدل إعادة توجيه المستخدم لصفحة Stripe Hosted، تم اعتماد **Stripe Payment Element داخل الموقع** بتصميم مطابق للثيم واستخدام نفس لوجو الفافيكون (`/LOGOM.png`). |

#### تحديثات إضافية (Store + Admin)

| التحديث                                    | الوصف                                                                                                                  |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| **Admin Promotions (Restaurants scope)**   | إنشاء promo code وربطه بمطاعم محددة عبر `restaurantIds` + تطبيق التحقق/الخصم في الـ checkout حسب المطعم.               |
| **Promo Validity (Start/End)**             | دعم `startsAt/endsAt` في إنشاء البرومو، وعرضها في الأدمن، مع فلترة “شغال ولسا مش منتهي”.                               |
| **Promo Notifications (English defaults)** | الافتراضي لإشعار البرومو أصبح بالإنجليزي (مع إمكانية تخصيص title/body من الأدمن).                                      |
| **Admin Promotions Active List UI**        | إضافة جدول “Active promotions” في صفحة الأدمن يعرض البروموهات الفعالة غير المنتهية + حالات loading/empty.              |
| **Admin Promotions Delete UI**             | إضافة زر حذف في جدول البروموهات (مع confirm + loading + refresh) داخل لوحة الأدمن.                                     |
| **Auth Token Scoping Fix**                 | منع إرسال admin token على مسارات المستخدم لتجنب Unauthorized في صفحات المطاعم (خصوصاً cart preload).                   |
| **Cart Invoice Currency**                  | عرض إجمالي/فاتورة الكارت باليورو EUR باستخدام `Intl.NumberFormat`.                                                     |
| **Home Redirect**                          | جعل `/` يفتح صفحة المطاعم `/restaurants` افتراضياً، مع إبقاء صفحة الهبوط متاحة على `/home`.                            |
| **Admin Dashboard Overview**               | إضافة API وواجهة Dashboard لعرض counts + pending actions (provider requests / support tickets / missing bank details). |
| **Providers Bank Details (Admin)**         | عرض بيانات البنك للمزود (IBAN/SWIFT) في صفحة Partner Places للأدمن بشكل masked افتراضياً + أزرار Copy.                 |

#### ملاحظات تشغيل مهمة

- **WhatsApp عبر Twilio** يتطلب sender مُفعّل للواتساب (أو sandbox). عدم تفعيل الرقم/المسار سيؤدي لأخطاء من Twilio حتى لو المفاتيح صحيحة.
- **Twilio Geo Permissions** قد تمنع إرسال SMS لبعض الدول حتى لو الكود صحيح.
- لا تضع أسرار Stripe/Twilio داخل التقرير؛ استخدم `.env` و `.env.example` فقط.

### Web Frontend

| التحديث                      | الوصف                                                                                                               |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Default Dark Theme**       | جعل الدارك مود هو الوضع الافتراضي على أول تحميل (First paint) + حفظ/تطبيق اختيار المستخدم بشكل واضح                 |
| **Theme Override Fix**       | تعديل CSS بحيث `prefers-color-scheme: dark` لا يتجاوز اختيار المستخدم عند تفعيل Light/Dark/High-contrast            |
| **Wolt-like Single Color**   | إزالة الخلفيات gradients في صفحات المتجر (Restaurants list/details) واستبدالها بخلفية سادة + accent cyan عبر tokens |
| **About/Landing Polish**     | إزالة بقايا gradients وتحسين التباين وربط الألوان بـ tokens بدل ألوان hardcoded                                     |
| **Build Verified**           | `wfrontend` build ناجح؛ يوجد تحذير واحد معروف (Leaflet CommonJS) بدون تأثير على التشغيل                             |
| **Provider System Settings** | صفحة `/provider/system-settings` محمية بإعادة إدخال كلمة المرور + i18n كامل                                         |
| **Provider Dashboard Menu**  | إدارة الأصناف والمنيو داخل الداشبورد (مودالات Add/Edit/Delete + صور + Availability + سعر EUR)                       |

### Frontend Web

| الميزة                        | الوصف                                                                         |
| ----------------------------- | ----------------------------------------------------------------------------- |
| **HTTP Interceptors**         | Interceptors موحدة (Auth + Retry + Loading + Errors) على مستوى التطبيق        |
| **Global Loading State**      | Overlay عام يظهر أثناء أي HTTP request عبر LoadingService                     |
| **Unified Error Handling**    | ErrorInterceptor + GlobalErrorHandler مع Toast موحد للأخطاء                   |
| **Landing Images Admin**      | صفحة الأدمن أصبحت لإدارة صور اللاندنج (5 Slots)                               |
| **About Images Admin**        | صفحة أدمن جديدة لإدارة صور صفحة About (5 Slots)                               |
| **Landing Page Dynamic Hero** | الهيرو سيكشن يدعم صورة خلفية ديناميكية مع نفس التقوس                          |
| **Drag & Drop Upload**        | رفع الصور بالسحب والإفلات أو الضغط للتصفح                                     |
| **Preview with Curve**        | معاينة الصورة مع نفس شكل التقوس السفلي                                        |
| **7 Languages Support**       | صفحة الهبوط تدعم 7 لغات (en, fr, lb, de, it, pt, es)                          |
| **WebSocket Real-time**       | تحديث فوري لصور اللاندنج (Hero + Why) باستخدام Socket.IO                      |
| **Notifications UI (MVP)**    | صفحة `/notifications` + badge في navbar + Mark as read/all + realtime         |
| **Orders UI (MVP)**           | داخل Profile: Order history + زر Track يفتح `/orders/:id/tracking` بخريطة     |
| **Web Console Warning Fix**   | إصلاح تحذير DevTools (إضافة `id/name` لحقل البحث في Landing)                  |
| **Perf Baseline (local)**     | LCP ~ 896ms، CLS ~ 0.23 (Needs improvement)                                   |
| **Cute Navbar Tabs**          | أزرار الـ Cute Navbar تعمل كتبديل لعرض الصور والنصوص                          |
| **Skeleton + Toast Loading**  | Skeleton + Lazy loading للصور + Loading toast عند البطء                       |
| **About Page UI**             | صفحة About جديدة بألوان Landing v2 + تداخل سيكشنات                            |
| **Restaurants Listing (MVP)** | قائمة مطاعم حقيقية من الـ API + بحث + فلتر Open Only                          |
| **Restaurant Details (MVP)**  | تبويبات التصنيفات + عرض الـ menu + Add to cart                                |
| **Cart + Checkout (MVP)**     | تعديل الكميات + promo validate + Checkout                                     |
| **Navbar Search/Cart Badge**  | البحث يفتح restaurants مع query + عداد السلة                                  |
| **Theme Fix (User Pages)**    | إصلاح الدارك مود ليشمل الصفحات (Profile/Legal/About/…)                        |
| **Theme RGB Helpers**         | إضافة `--ink-rgb`/`--paper-rgb` وتطبيقها في CSS overlays                      |
| **Placeholder Pages Themed**  | صفحات Driver/Provider أصبحت تستخدم ألوان الثيم                                |
| **User Tokens Flow**          | تسجيل دخول المستخدم أصبح عبر `/auth/user/login` + تخزين access/refresh tokens |
| **User Logout Revocation**    | logout يعمل best-effort على `/auth/user/logout` ثم يمسح session + tokens      |

### تحديثات مارس 2026 (v0.0.17+)

#### Backend

| التحديث                    | الوصف                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Short Order IDs**        | أوردرات تُعرض بكود 3 أرقام + حرف (مثال: "537B") بدل UUID طويل — `shortOrderId()` في backend + frontend |
| **SMS Order Confirmation** | رسالة SMS بتفاصيل كاملة (اسم المطعم، العناصر، الأسعار، عنوان التوصيل، الإجمالي)                        |
| **Driver Applications**    | نظام تقديم طلبات السائقين (DriverApplicationEntity + CRUD + admin review)                              |
| **PostgreSQL Security**    | تأمين PostgreSQL: `listen_addresses = 'localhost'` + إزالة `0.0.0.0/0` من pg_hba.conf + UFW deny 5432  |
| **DATABASE_HOST Fix**      | تصحيح `DATABASE_HOST` من IP عام إلى `localhost` في `.env` على VPS                                      |

#### Web Frontend

| التحديث                        | الوصف                                                                               |
| ------------------------------ | ----------------------------------------------------------------------------------- |
| **Provider Audio Alerts**      | نظام صوت Web Audio API: 3 نغمات تصاعدية للطلبات الجديدة + نغمة واحدة لرسائل الأدمن  |
| **Thermal Receipt Printing**   | طباعة حرارية (80mm) عبر iframe مخفي — أسرع من `window.open()` على التابلت           |
| **Invoice Modal**              | شعار Moien Delivery + اسم المطعم + إيميل وهاتف العميل + تاريخ منسق                  |
| **Short Order IDs (Frontend)** | `shortOrderId()` يعرض 3 أرقام + حرف بدل 8 حروف UUID                                 |
| **Print Receipt Design**       | خط Monospace + فواصل منقطة + `@page size: 80mm` + عناصر بتنسيق `1x Name ... €price` |
| **Dead Code Cleanup**          | إزالة كود ميت + تحسين أداء البحث في المنيو                                          |
| **Scroll-to-Top**              | زر تمرير لأعلى في صفحات الفئات                                                      |
| **Driver Page Redesign**       | إعادة تصميم صفحة السائقين                                                           |

#### Infrastructure

| التحديث                 | الوصف                                                          |
| ----------------------- | -------------------------------------------------------------- |
| **PostgreSQL Hardened** | PostgreSQL يستمع فقط على 127.0.0.1 (ليس 0.0.0.0)               |
| **UFW Updated**         | حذف rule لسماح IP محدد بالمنفذ 5432 + إضافة deny 5432/tcp صريح |
| **Node Symlink Fix**    | إصلاح `/usr/bin/node` symlink بعد إعادة تشغيل VPS (EACCES)     |
| **PM2 Restart**         | إعادة تشغيل PM2 مع `--update-env` بعد تغيير DATABASE_HOST      |

---

## حالة التطبيق المحمول (Mobile)

| المكون                          | الحالة                                                                                                         | الملاحظات                                                   |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| Flutter App (mfrontend)         | ❌ لم يبدأ                                                                                                     | المجلد يحتوي فقط على scaffold الافتراضي. لا يوجد تطبيق فعلي |
| **Partner Places Admin**        | صفحة `/admin/partner-places` لإدارة الطلبات + Actions (Approve/Details/Reject/Message/Delete)                  |
| **Partner Places Real-time**    | تحديث لحظي لجدول Partner Places عبر Socket.IO (listen `partner-places.changed` ثم reload)                      |
| **Delete Provider Master Code** | عند حذف Provider/Request: يظهر Modal لكتابة Master Code + عرض Attempts المتبقية + Lock لمدة دقائق مع Countdown |
| **Provider Portal UX**          | منع تسجيل Provider أثناء وجود جلسة User + رسالة إنجليزية ضمن نظام ترجمة 7 لغات                                 |
| **Provider Navbar Label**       | شارة “Provider” تظهر في navbar عند وجود جلسة Provider                                                          |

### Technical Details

```
Backend:
  - GET    /api/v1/health/services                      (health check: api/db/redis)
  - POST   /api/v1/site-settings/hero-image/upload  (رفع صورة)
  - GET    /api/v1/site-settings/hero-image         (جلب الصورة)
  - DELETE /api/v1/site-settings/hero-image         (حذف الصورة)

  - GET    /api/v1/site-settings/landing-images                   (جلب كل الصور)
  - POST   /api/v1/site-settings/landing-images/:slot/upload      (رفع صورة slot)
  - DELETE /api/v1/site-settings/landing-images/:slot             (حذف صورة slot)

  - GET    /api/v1/partner-places/requests                        (Admin: list)
  - POST   /api/v1/partner-places/requests                         (Public: create request)
  - POST   /api/v1/partner-places/requests/:id/approve            (Admin)
  - POST   /api/v1/partner-places/requests/:id/request-details    (Admin)
  - POST   /api/v1/partner-places/requests/:id/reject             (Admin)
  - POST   /api/v1/partner-places/requests/:id/message            (Admin)
  - DELETE /api/v1/partner-places/requests/:id                    (Admin: delete provider/request)

  - ملاحظة: حذف الـ Provider/Request من واجهة الأدمن يتطلب التحقق من الـ master code عبر `POST /__access/check` قبل تنفيذ الحذف (UI enforced).

  - GET    /api/v1/site-settings/about-images                     (جلب كل صور About)
  - POST   /api/v1/site-settings/about-images/:slot/upload        (رفع صورة About slot)
  - DELETE /api/v1/site-settings/about-images/:slot               (حذف صورة About slot)

WebSocket:
  - Namespace: /site-settings
    - Event: hero-image.updated { url: string | null }
    - Event: landing-image.updated { slot: 'hero'|'whyFresh'|'whyFast'|'whySupport'|'whyQuality', url: string | null }
    - Event: about-image.updated { slot: 'aboutHero'|'aboutStory'|'aboutFeature1'|'aboutFeature2'|'aboutFeature3', url: string | null }
  - Namespace: /partner-places
    - Event: partner-places.changed { kind: 'created'|'approved'|'needs_info'|'rejected'|'updated'|'deleted', id?: string, at: string }
  - Namespace: /notifications
    - Event: notifications.created (NotificationEntity)
    - Event: notifications.read { userId: string, id: string }
    - Event: notifications.readAll { userId: string }

  - Namespace: /admin-drivers
    - Event: adminDrivers.snapshot { ts: string, drivers: any[] }

  - Namespace: /driver
    - Event: driver.orders { ts: string, orders: any[] }
    - Event: driver.orderAssigned { orderId: string | null, ts: string }
    - Event: driver.inbox { ts: string, messages: any[] }
    - Event: driver.message { id?: string, ts: string, from: 'admin', message: string }
    - Event: driver.forcedLogout { ts: string, reason: string }

  - Namespace: /user-orders
    - Event: userOrders.orders { ts: string, orders: any[] }
    - Event: userOrders.order.tracking { ts: string, tracking: any }

Frontend:
  - /admin/hero-settings                            (صفحة الإدارة)
  - /admin/about-images                             (صفحة إدارة صور About)
  - /admin/partner-places                           (صفحة إدارة Partner Places)
  - /about-us                                       (صفحة About)
  - /restaurants                                    (قائمة المطاعم)
  - /restaurants/:id                                (تفاصيل المطعم + المنيو)
  - /cart                                           (السلة + إتمام الطلب)
  - صفحة الإدارة تعرض 5 Slots (Hero + 4 صور Why Choose Us)
  - الهيرو يعرض صورة أو اللون الأحمر الافتراضي
  - تحديث لحظي عند رفع/حذف الصور من الأدمن

Build/Budgets:
  - Angular production budgets: anyComponentStyle warning=10kB / error=20kB
  - تم تفادي تحذيرات الـ CSS budget في build

Security (Backend):
  - Rate limiting (global): ThrottlerGuard (env: THROTTLE_TTL_SEC, THROTTLE_LIMIT)
  - CORS whitelist: `CORS_ORIGINS` (comma-separated)
  - Helmet enabled (CSP disabled by default)
  - CSRF middleware (double-submit cookie): enable via `CSRF_ENABLED=true`
  - Root access gate (in-memory): code gate + lockouts + DevTools/unusual bans (best-effort)

Root Access Gate (Backend):
  - GET  /__access/status
  - POST /__access/check
  - POST /__access/devtools
  - Env:
    - SERVER_ACCESS_CODE (default: 123456)
    - SERVER_ACCESS_MAX_ATTEMPTS (default: 3)
    - SERVER_ACCESS_BAN_MS (default: 300000)
    - SERVER_ACCESS_DEVTOOLS_BAN_MS (default: 600000)
    - SERVER_ACCESS_UNUSUAL_BAN_MS (default: 3600000)
    - SERVER_ACCESS_UNUSUAL_WINDOW_MS, SERVER_ACCESS_UNUSUAL_MAX_REQUESTS

Database (Backend):
  - Run migrations: `npm run db:migrate:run`
  - Show migrations: `npm run db:migrate:show`
  - Revert last migration: `npm run db:migrate:revert`
  - Seed dev data: `npm run db:seed`
  - Seed dry-run: `npm run db:seed:dry`
  - Safety: `DATABASE_SYNC` is ignored in production (sync=false)
  - Optional: auto-run migrations on boot via `DATABASE_MIGRATIONS_RUN=true`

Auth (Backend):
  - POST /api/v1/auth/user/login
  - POST /api/v1/auth/user/refresh
  - POST /api/v1/auth/user/logout
  - GET  /api/v1/auth/user/sessions (Bearer)
  - POST /api/v1/auth/user/sessions/:id/revoke (Bearer)
  - POST /api/v1/auth/admin/login
  - POST /api/v1/auth/admin/refresh
  - POST /api/v1/auth/admin/logout
  - GET  /api/v1/auth/admin/sessions (Bearer)
  - POST /api/v1/auth/admin/sessions/:id/revoke (Bearer)

Theme notes:
  - السبب الأساسي للمشكلة كان وجود ألوان hardcoded داخل CSS لبعض الصفحات (خصوصاً rgba للأبيض)
  - تم تحويلها لاستخدام متغيرات CSS theme-reactive عبر `--ink-rgb`/`--paper-rgb`
```

### تحديثات إضافية (حتى 26 فبراير 2026)

#### Backend

| التحديث                          | الوصف                                                                                                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Ratings Module**               | وحدة جديدة `ratings/`: تقييم المطعم بعد الطلب + تقييم المنصة + تقييمات المزودين. كيانات: `PlatformRatingEntity` + `RestaurantRatingEntity`. |
| **Partner Notifications Module** | وحدة جديدة `partner-notifications/`: إشعارات خاصة بالمزود + `PartnerJwtAuthGuard` + WebSocket Gateway لتحديثات لحظية.                       |
| **Promotional Offers API**       | نظام عروض ترويجية مرئية (بانرات) منفصل: عناوين ثنائية اللغة + صور + نسبة خصم + شروط مرنة + ربط بمطاعم.                                      |

#### Web Frontend

| التحديث                                      | الوصف                                                                                                                                   |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Restaurants Page Redesign**                | عنوان "Our Providers" + بحث فوري + شريط تصنيفات متحرك (20 تصنيف بألوان وأيقونات) + شريط عروض ترويجية متحرك + hover pause.               |
| **Restaurant Detail Offer Banners**          | عرض أفضل عرض ترويجي للمطعم كبانر (`bestOffer` computed signal).                                                                         |
| **Promotional Offers v2 (Admin)**            | إنشاء/تعديل/حذف عروض بانر مرئية + عناوين EN/AR + شروط مرنة (min_order/free_delivery/first_order/min_items) + ربط بمطاعم + صلاحية زمنية. |
| **International Phone Calling Codes**        | قائمة منسدلة لكود الاتصال الدولي عند التسجيل (داخل مودال Navbar) + دعم 30+ دولة + أعلام.                                                |
| **Phone Digit Count Validation**             | التحقق من عدد أرقام الهاتف حسب الدولة المختارة (مثلاً: مصر=10، لوكسمبورغ=6-9).                                                          |
| **Auto-Login After Register**                | تسجيل الدخول تلقائياً بعد التسجيل الناجح بدون إعادة إدخال البيانات.                                                                     |
| **Global 401/403 Error Toast Suppression**   | `ErrorInterceptor` يتجاهل أخطاء 401/403 عالمياً — لم تعد تظهر كـ Toast مزعج.                                                            |
| **GlobalErrorHandler HttpErrorResponse Fix** | `GlobalErrorHandler` يتجاهل `HttpErrorResponse` و `"Unauthorized"` لمنع Toast مكرر.                                                     |
| **X-Skip-Error-Toast Header**                | إضافة header للطلبات الاختيارية (offers, cart preload, admin) لمنع toast حتى على أخطاء أخرى.                                            |

---

## 📞 الخطوات القادمة

1. **فوراً**: ضبط thresholds الخاصة بـ Unusual Activity في بيئة التطوير لتقليل false positives (env المذكورة أعلاه)
2. **هذا الأسبوع**: إضافة اختبارات e2e بسيطة لمسارات Orders (me/tracking) + Notifications (read/readAll)
3. **التالي مباشرة**: Delivery Zones + Real-time tracking متقدم (خريطة)
4. **تحسين سريع**: تحسين observability للـ bans (تجميع counters في Monitoring Logs stats)
5. **اختياري لاحقاً**: جعل bans/attempts persistent (Redis/DB) بدل in-memory لو احتجنا سلوك ثابت بعد restart
6. **تطبيق الموبايل**: البدء في تطوير تطبيق Flutter للعملاء والسائقين

---

## 🆕 آخر التحديثات (v0.0.15)

### Backend

| التحديث                        | الوصف                                                                                                                  |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| **Geocode Proxy**              | إضافة `GeocodeController` كبروكسي لـ Nominatim (reverse/search) لتجنب CORS — تستخدمها الواجهة بدلاً من الاتصال المباشر |
| **Stripe Customer Resilience** | `getOrCreateCustomer()` يتحقق من وجود العميل في Stripe قبل استخدامه — يعيد الإنشاء تلقائياً عند الانتقال test→live     |
| **Seed Demo Data**             | بيانات تجريبية شاملة: 25 مطعم + 5 متاجر + ~120 تصنيف + ~240 منيو + 6 عروض + كود WELCOME10                              |
| **Promotional Offers Seed**    | 6 عروض ترويجية مع صور وشروط متنوعة (free_delivery / min_order / first_order / min_items)                               |

### Web Frontend

| التحديث                         | الوصف                                                                                                                             |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Carousel Redesign**           | تحويل الشرائط المتحركة (marquees) إلى كاروسيلات بأسهم يمين/يسار + سحب بالماوس + تمرير تلقائي لا نهائي عبر `requestAnimationFrame` |
| **Infinite Loop Carousel**      | تكرار العناصر (Set A + Set B) مع إعادة ضبط تلقائية عند نقطة النصف — تمرير سلس بلا انقطاع                                          |
| **Offer Card Navigation**       | الضغط على بطاقة عرض ترويجي ينقل المستخدم مباشرة لصفحة المطعم بدلاً من فتح modal                                                   |
| **Leaflet Map Fix**             | إصلاح `t.map is not a function` في Driver Dashboard عبر التعامل مع `.default` wrapper من esbuild                                  |
| **Registration Simplification** | تبسيط التسجيل: إزالة خطوة التحقق — تسجيل فوري بالاسم والبريد وكلمة المرور                                                         |
| **Open/Closed Badges**          | شارات مرئية على بطاقات المطاعم حسب ساعات العمل                                                                                    |
| **Responsive Mobile**           | تحسين التصميم على الموبايل + أزرار footer ثابتة + session timeout UX                                                              |
| **Location Quick-Save**         | زر حفظ سريع للموقع في صفحة العناوين مع خريطة Leaflet                                                                              |
| **Geocode via Proxy**           | تحديث 5 ملفات لاستخدام geocode proxy من الخادم بدلاً من Nominatim مباشرة                                                          |

### إصلاحات الإنتاج

| الإصلاح                 | الوصف                                                                         |
| ----------------------- | ----------------------------------------------------------------------------- |
| **Promo 404 Fix**       | إصلاح 404 على `/api/v1/promotional-offers/active` بسبب بناء قديم (stale dist) |
| **Stale Chunks Fix**    | حذف ملفات JavaScript القديمة ونشر 64 ملف جديد                                 |
| **Stripe Customer Fix** | تصفية `stripeCustomerId` القديمة من DB + إعادة إنشاء تلقائية                  |
| **Permission Fix**      | إصلاح صلاحيات `dist/` المملوكة لـ root بعد SCP                                |

---

## 🆕 آخر التحديثات (v0.0.17)

### Backend

| التحديث                             | الوصف                                                                                                |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Multi-Restaurant Cart**           | `GET /orders/me/carts` — جلب كل سلات المستخدم النشطة                                                 |
| **Cart Enrichment**                 | `enrichCartWithRestaurant()` helper يُرفق `restaurantName` و `restaurantLogo` لكل استجابة سلة        |
| **Cart Enrichment (all mutations)** | مُطبّق على `getOrCreateCart()`, `addItem()`, `updateCartItem()`, `listMyCarts()`                     |
| **Database Schema**                 | إضافة أعمدة `paymentMethod` و `paypalOrderId` لجدول الطلبات                                          |
| **PayPal Integration**              | تكامل PayPal REST SDK (Sandbox + Live): `POST /orders/paypal-create` و `POST /orders/paypal-capture` |
| **Free Delivery (New Users)**       | `isNewUserFreeDelivery()` يطبق توصيل مجاني تلقائي لأول 30 يوم من التسجيل في 4 مسارات checkout        |

### Web Frontend

| التحديث                           | الوصف                                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------ |
| **CartSidebarComponent**          | مكون عالمي (FAB + sidebar) يعرض سلات مجمّعة حسب المطعم مع أزرار تحكم بالكمية + checkout لكل مطعم |
| **CartStateService (Multi-Cart)** | `cartMap` signal (خريطة سلات حسب مطعم) + `allCarts` computed + `loadAllCarts()` + `totalCount`   |
| **Preview Modal**                 | مودال معاينة صور الأطباق: tap ℹ → صورة بقياس كامل + اسم + وصف + سعر                              |
| **Touch Gestures**                | pinch-to-zoom (1x–5x) + pan بإصبع واحد + double-tap (1x↔2.5x) + mouse wheel + mouse drag         |
| **Item Actions in Copy Area**     | نقل أزرار Add + Info من فوق الصورة إلى منطقة النص (`.rd-item-copy`)                              |
| **Step-Based Marquee**            | كاروسيل بخطوات سلسة (JS setInterval + CSS transition cubic-bezier) + أسهم + touch/swipe          |
| **PayPal Checkout Button**        | زر PayPal في صفحة Checkout مع redirect flow كامل                                                 |
| **Free Delivery Banner**          | بانر أخضر "توصيل مجاني للأعضاء الجدد!" في صفحة تفاصيل المطعم                                     |
| **About Page CTA**                | زرين: "تسجيل كمطعم" + "تقديم كسائق" (WhatsApp)                                                   |
| **Category Images (Icons8)**      | صور ملونة من Icons8 Color CDN بدل Font Awesome                                                   |

### DevOps / إصلاحات الإنتاج

| الإصلاح                       | الوصف                                                                   |
| ----------------------------- | ----------------------------------------------------------------------- |
| **Backend Deploy Fix**        | إعادة بناء `dist/` يدوياً + إصلاح أمر PM2 restart في سكربت النشر        |
| **Nginx SPA Cache Fix**       | إضافة `no-cache` لـ `index.html` في nginx config                        |
| **Nginx Root vs Browser Fix** | إصلاح مشكلة أن nginx يخدم من root dir بينما الملفات ترفع في `/browser/` |

---

<div align="center">

**📝 آخر تحديث: يوليو 2026**

[![Made with ❤️](https://img.shields.io/badge/Made%20with-❤️-red.svg)]()
[![For Moien Delivery](https://img.shields.io/badge/For-Moien%20Delivery-orange.svg)]()

</div>
