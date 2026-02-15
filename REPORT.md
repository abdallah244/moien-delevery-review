# 📊 تقرير حالة المشروع | Project Status Report

<div align="center">

[![Version](https://img.shields.io/badge/Version-v0.0.13-orange.svg)]()
[![Status](https://img.shields.io/badge/Status-قيد%20التطوير-yellow.svg)]()
[![Last Updated](https://img.shields.io/badge/آخر%20تحديث-فبراير%202026-blue.svg)]()

**📅 تاريخ التقرير: 15 فبراير 2026**

</div>

---

## 📋 جدول المحتويات

- [ملخص تنفيذي](#-ملخص-تنفيذي)
- [جرد المشروع (Inventory)](#-جرد-المشروع-inventory)
- [المشاكل الحرجة](#-المشاكل-الحرجة-critical-issues)
- [الأشياء التي تحتاج إصلاح](#-الأشياء-التي-تحتاج-إصلاح-bugs--fixes)
- [الأشياء التي تحتاج تحسين](#-الأشياء-التي-تحتاج-تحسين-improvements)
- [أجزاء موجودة لكن غير مستخدمة/غير مكتملة](#-أجزاء-موجودة-لكن-غير-مستخدمةغير-مكتملة)
- [الميزات الناقصة](#-الميزات-الناقصة-missing-features)
- [مميزات تميزنا عن المنافسين](#-مميزات-تميزنا-عن-المنافسين-competitive-advantages)
- [خريطة الطريق المقترحة](#-خريطة-الطريق-المقترحة)
- [آخر التحديثات](#-آخر-التحديثات-v0013)

---

## 📈 ملخص تنفيذي

### نسبة الإنجاز الحالية

| المكون           | نسبة الإنجاز | الحالة         |
| ---------------- | ------------ | -------------- |
| 🖥️ Backend API   | 85%          | 🟡 قيد التطوير |
| 🌐 Web Frontend  | 76%          | 🟡 قيد التطوير |
| 📱 Mobile App    | 0%           | 🔴 لم يبدأ     |
| 🗄️ Database      | 78%          | 🟡 قيد التطوير |
| 📚 Documentation | 72%          | 🟡 متوسط       |
| 🧪 Testing       | 12%          | 🔴 ضعيف        |

### ما تم إنجازه ✅

| المكون   | التفاصيل                                                                                                                                                                              |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Backend  | 11 وحدة فعلياً في `backend/src/modules`: Admin Staff / Users / Auth / Payments / Support Tickets / Site Settings / Restaurants / Orders / Promotions / Notifications / Partner Places |
| Backend  | Restaurants + Restaurant Categories + Menu Items (CRUD + filtering)                                                                                                                   |
| Backend  | Orders (Cart) + Checkout (promoCode + discount) + Tracking endpoint للمستخدم                                                                                                          |
| Backend  | Promotions (Validate + Redemption tracking)                                                                                                                                           |
| Backend  | Notifications (DB + Socket.IO realtime عبر namespace /notifications)                                                                                                                  |
| Backend  | CommonModule: خدمات مشتركة (Security/Performance/Monitoring/Storage/Communication/Realtime...) + Redis cache اختياري عبر env                                                          |
| Backend  | إعداد PostgreSQL + TypeORM                                                                                                                                                            |
| Backend  | تكامل Stripe + Cloudinary                                                                                                                                                             |
| Frontend | Angular Standalone + Interceptors + خدمات Session/Toast/Language + تكامل Socket.IO في صفحات محددة                                                                                     |
| Frontend | نظام المصادقة + لوحة تحكم الأدمن                                                                                                                                                      |
| Frontend | صفحة الهبوط (Landing Page) مع 7 لغات + تحسينات أداء (Skeleton/Lazy)                                                                                                                   |
| Frontend | صفحات المتجر (MVP): Restaurants List + Restaurant Details/Menu + Cart + Checkout                                                                                                      |
| Frontend | Notifications UI (MVP): قائمة + badge + Mark as read + realtime                                                                                                                       |
| Frontend | إدارة صور اللاندنج (Hero + 4 صور Why Choose Us) عبر Cloudinary                                                                                                                        |
| Frontend | صفحة About (Wolt-inspired) مع تقليل السكاشن + تحسينات UI/Contrast                                                                                                                     |
| Frontend | إدارة صور صفحة About عبر الأدمن (Slots مستقلة عن اللاندنج)                                                                                                                            |
| Frontend | أزرار الـ Cute Navbar تعمل كتبديل (Tabs) لعرض الصور والنصوص ديناميكياً                                                                                                                |
| Frontend | تحسين نظام الثيم: Dark افتراضي + منع `prefers-color-scheme` من تجاوز اختيار المستخدم + تعيين `data-theme` على عنصر `html`                                                             |
| Frontend | توحيد ستايل المتجر (Restaurants list/details): إزالة gradients + ربط الألوان بـ tokens الثيم (accent cyan)                                                                            |
| Frontend | Partner Places (Admin): عرض الطلبات + Actions (Approve/Needs info/Reject/Message/Delete) + تحديث لحظي                                                                                 |
| Frontend | Partner Portal (Provider): تسجيل دخول + منع تعارض الجلسات (log out first) + ترجمة الرسالة + شارة Provider في navbar                                                                   |
| Frontend | Provider System Settings: صفحة إعدادات (General/Hours/Branches/Danger Zone) محمية بإعادة إدخال كلمة المرور + Unlock قصير عبر sessionStorage                                           |
| Frontend | Provider Dashboard: إضافة إدارة الأصناف (Categories) وعناصر المنيو (Menu Items) (Add/Edit/Delete) مع رفع صورة العنصر + Availability + ربط Category + عرض السعر باليورو                |
| Frontend | Admin Promotions: إنشاء Promo codes + ربطها بمطاعم محددة + صلاحية (start/end) + إرسال إشعارات لمستخدمين محددين + عرض قائمة Active (غير منتهية)                                        |
| Frontend | Store UX: جعل الصفحة الرئيسية للمستخدم تفتح `/restaurants` تلقائياً + عرض إجمالي/فاتورة الكارت بعملة EUR                                                                              |
| Backend  | Provider/Partner APIs: `GET /partner-places/me` + تحديثات `PATCH /partner-places/me/request` + رفع صورة المطعم + CRUD للأصناف/المنيو scoped على مطعم الـ Provider                     |
| Docs     | توثيق موجود (ملفات في الجذر + مجلد `docs/`) لكن يحتاج مراجعة/توحيد مع الواقع الحالي                                                                                                   |

---

## 🧾 جرد المشروع (Inventory)

> هذا القسم يوثق ما هو موجود فعلاً في الريبو (Modules/Pages/Services) لتحديد الناقص/غير المستخدم بدقة.

### Backend (NestJS)

- Modules (مُستخدمة داخل `AppModule`):
  - `admin-staff`, `users`, `auth`, `payments`, `support-tickets`, `site-settings`, `restaurants`, `orders`, `promotions`, `notifications`, `partner-places`.
- CommonModule (Global):
  - Security/Performance/Monitoring/Storage/Communication/Realtime…
  - Redis cache اختياري عبر `CACHE_REDIS_ENABLED` (fallback إلى in-memory).

### Web Frontend (Angular)

- Pages/Areas (موجودة):
  - Landing, Restaurants, Cart/Checkout, User (Profile + Legal), Admin (dashboard + site settings + partner places), Provider portal.
- Driver pages: موجودة لكن ما زالت “coming soon/MVP”.

### Mobile (Flutter)

- هيكل Flutter موجود لكن لا يوجد تطبيق فعلي مبني على الـ features بعد.

### تقدير نسب الإنجاز التفصيلية (Estimates)

> هذه نسب تقريبية مبنية على وجود الـ endpoints/الصفحات والـ flows الأساسية، وليست مبنية على تغطية اختبارات.

#### Backend Modules

| الوحدة          | نسبة الإنجاز | ملاحظات مختصرة                                                                                                 |
| --------------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| Auth            | 82%          | Login/Refresh/Logout/Sessions موجودة؛ OAuth غير موجود                                                          |
| Users           | 80%          | CRUD أساسي + تحقق Email/Phone (OTP عبر SMS/WhatsApp) + عناوين + وسائل دفع (Stripe)                             |
| Restaurants     | 80%          | CRUD + categories + menu items (MVP)                                                                           |
| Orders          | 72%          | Cart + Checkout + Tracking للمستخدم؛ لا يوجد Driver assignment/status lifecycle كامل                           |
| Payments        | 75%          | publishable key + setup intent + webhook receiver (signature verify)؛ لا يوجد settlement flow كامل             |
| Promotions      | 75%          | Admin CRUD + صلاحية start/end + ربط بمطاعم محددة + Validate + تطبيق الخصم + إرسال إشعارات للمستخدمين           |
| Notifications   | 70%          | DB + Socket.IO + Delete/Clear للـ read + TTL cleanup للـ read؛ Push غير موجود                                  |
| Partner Places  | 92%          | Workflow كامل + Provider Portal (Me endpoints) + فروع + حذف الحساب + إدارة المنيو/الأصناف + رفع صور Cloudinary |
| Site Settings   | 80%          | صور landing/about + CRUD إعدادات أساسي                                                                         |
| Support Tickets | 60%          | إنشاء/عرض/رد؛ يحتاج SLA/attachments/workflow متقدم                                                             |
| Admin Staff     | 60%          | Login + CRUD؛ صلاحيات/roles متقدمة غير موجودة                                                                  |

#### Web Areas

| الجزء             | نسبة الإنجاز | ملاحظات مختصرة                                                                          |
| ----------------- | ------------ | --------------------------------------------------------------------------------------- |
| Landing/About     | 90%          | UI قوي + i18n؛ بعض اللغات تحتوي سلاسل غير مترجمة بالكامل                                |
| Restaurants/Store | 82%          | Browse + details/menu + cart + تحسينات Theme/UI + homepage redirect to restaurants      |
| Checkout          | 68%          | MVP checkout + promo + add card via Stripe Payment Element                              |
| User Profile      | 70%          | بيانات + عناوين + payment methods (Stripe)                                              |
| Admin             | 72%          | إدارة صور + Partner Places admin + Promotions admin (create/scope/notify + active list) |
| Provider Portal   | 88%          | Login + System Settings gate + Dashboard + إدارة المنيو/الأصناف                         |
| Driver            | 10%          | Placeholder فقط                                                                         |

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

| المشكلة                | الخطورة  | الوصف                                                         |
| ---------------------- | -------- | ------------------------------------------------------------- |
| **Refresh Token**      | ✅ تم    | Refresh token opaque + rotation عبر sessions في DB            |
| **Token Blacklisting** | ✅ تم    | إبطال الجلسة (revoke session) يبطل access tokens المرتبطة بها |
| **Session Management** | ✅ تم    | جدول sessions + list/revoke endpoints لتتبع الجلسات النشطة    |
| **لا يوجد OAuth**      | 🟡 متوسط | تسجيل الدخول بـ Google/Apple/Facebook                         |

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

| #   | المشكلة                                            | الملف/الموقع                                                                                    | الأولوية |
| --- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------- | -------- |
| 1   | لا يوجد Error Handling موحد                        | `wfrontend/src/app/interceptors/error.interceptor.ts` + `utils/global-error-handler.ts`         | ✅ تم    |
| 2   | لا يوجد Loading State عام                          | `wfrontend/src/app/services/loading.ts` + `components/global-loading/*`                         | ✅ تم    |
| 3   | لا يوجد HTTP Interceptor موحد (Auth/Errors/Retry)  | `wfrontend/src/app/app.config.ts` + `wfrontend/src/app/interceptors/*`                          | ✅ تم    |
| 4   | بعض الصفحات لا تتأثر بتغيير الثيم بسبب ألوان ثابتة | صفحات User/Store                                                                                | ✅ تم    |
| 5   | لا يوجد PWA support                                | -                                                                                               | 🟡 منخفض |
| 6   | لا يوجد SEO optimization                           | -                                                                                               | 🟡 منخفض |
| 7   | تحذير DevTools: بعض حقول الإدخال بدون `id/name`    | `wfrontend/src/app/pages/landing/landing.v2.html`                                               | ✅ تم    |
| 8   | بعض الترجمات غير مكتملة (سلاسل EN داخل لغات أخرى)  | `wfrontend/src/app/services/language.ts`                                                        | 🟡 متوسط |
| 9   | إضافة وسيلة دفع (Add Card)                         | `wfrontend/src/app/pages/user/profile/*` + `backend/src/modules/users/users-billing.service.ts` | ✅ تم    |
| 10  | اختبار واجهة الويب يفشل بسبب توقع وجود `h1`        | `wfrontend/src/app/app.spec.ts`                                                                 | 🟡 متوسط |

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

| #   | التحسين                   | الوصف                      | الأولوية |
| --- | ------------------------- | -------------------------- | -------- |
| 1   | **Docker Compose**        | بيئة تطوير متكاملة         | 🔴 عالي  |
| 2   | **CI/CD Pipeline**        | GitHub Actions للنشر الآلي | 🔴 عالي  |
| 3   | **Environment Variables** | إدارة أفضل للمتغيرات       | 🟠 متوسط |
| 4   | **Health Checks**         | مراقبة حالة الخدمات        | 🟠 متوسط |
| 5   | **Kubernetes Manifests**  | للنشر على السحابة          | 🟡 منخفض |

---

## 🧩 أجزاء موجودة لكن غير مستخدمة/غير مكتملة

| الجزء                         | أين موجود                                                         | الحالة الحالية                                     | ملاحظات عملية                                                                                                     |
| ----------------------------- | ----------------------------------------------------------------- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Search/Analytics/Queue/Backup | `backend/src/common/services/*` + صفحات status في `AppController` | 🟡 موجودة كـ services لكن غير مكشوفة كـ APIs فعلية | يوجد صفحات status فقط، ولا توجد Controllers عامة لـ search/analytics/queue/backup حتى الآن.                       |
| SMS/WhatsApp (Twilio)         | `backend/src/common/services/communication/*`                     | 🟡 تكامل موجود ويعتمد على إعداد الحساب             | SMS/WhatsApp يعملان عبر Twilio؛ WhatsApp يتطلب sender مُفعّل (أو Sandbox). SMS قد يتطلب Geo Permissions لكل دولة. |
| Driver Web Pages              | `wfrontend/src/app/pages/driver/*`                                | 🔴 Placeholder                                     | صفحات “coming soon/MVP” بدون مهام توصيل/تتبع/أرباح حقيقية.                                                        |
| Mobile App                    | `mfrontend/`                                                      | 🔴 Skeleton                                        | مشروع Flutter موجود (platforms + build) لكن بدون features فعلية.                                                  |
| Docs Consistency              | `CATALOG.md` + `ARCHITECTURE.md`                                  | 🟡 يحتاج مراجعة                                    | تم تحديثهما ليعكسا الواقع الحالي (v0.0.13) لكن يحتاجان مراجعة دورية مع كل إصدار جديد.                             |

## 📦 الميزات الناقصة (Missing Features)

### الوحدات الأساسية (Core Modules)

| #   | الوحدة                | الوصف                        | الأولوية | الحالة                                                                            |
| --- | --------------------- | ---------------------------- | -------- | --------------------------------------------------------------------------------- |
| 1   | 🍴 **Restaurants**    | إدارة المطاعم والقوائم       | 🔴 حرج   | ✅ تم (Backend + Web MVP)                                                         |
| 2   | 🛒 **Orders**         | نظام الطلبات والسلة          | 🔴 حرج   | ✅ تم (Cart + Checkout MVP)                                                       |
| 3   | 🚗 **Drivers**        | إدارة السائقين والتوصيل      | 🔴 حرج   | 🟡 Web placeholder فقط (بدون Backend/Workflow)                                    |
| 4   | 📍 **Delivery Zones** | مناطق التوصيل والتغطية       | 🔴 حرج   | ❌ لم يبدأ                                                                        |
| 5   | 🏷️ **Categories**     | تصنيفات المطاعم والأطباق     | 🟠 عالي  | 🟡 جزئي (Restaurant Categories)                                                   |
| 6   | ⭐ **Reviews**        | نظام التقييمات والمراجعات    | 🟠 عالي  | ❌ لم يبدأ                                                                        |
| 7   | 🎁 **Promotions**     | الكوبونات والعروض            | 🟠 عالي  | ✅ تم (Admin CRUD + صلاحية start/end + ربط مطاعم + validate/apply + notify users) |
| 8   | 🔔 **Notifications**  | الإشعارات (Push, Email, SMS) | 🟠 عالي  | 🟡 DB + Socket.IO ✅ / Push ❌ / SMS/WhatsApp (Twilio) ✅\*                       |

\* ملاحظة: التكامل موجود، لكن يحتاج إعداد Twilio (sender/geo permissions) حسب الدولة.

### الميزات الإضافية (Additional Features)

| #   | الميزة                     | الوصف                       | الأولوية |
| --- | -------------------------- | --------------------------- | -------- |
| 1   | 🔍 **Search & Filters**    | بحث متقدم وفلاتر            | 🟠 عالي  |
| 2   | 🗺️ **Real-time Tracking**  | تتبع الطلب على الخريطة      | 🟠 عالي  |
| 3   | 💬 **Chat System**         | محادثة بين المستخدم والسائق | 🟠 عالي  |
| 4   | 📊 **Analytics Dashboard** | لوحة إحصائيات للمطاعم       | 🟠 عالي  |
| 5   | 🔄 **Order Scheduling**    | جدولة الطلبات مسبقاً        | 🟡 متوسط |
| 6   | 👥 **Group Orders**        | طلبات جماعية                | 🟡 متوسط |
| 7   | 🎯 **Recommendations**     | توصيات ذكية                 | 🟡 متوسط |
| 8   | 💰 **Wallet System**       | محفظة إلكترونية             | 🟡 متوسط |
| 9   | 🏆 **Loyalty Program**     | برنامج الولاء والنقاط       | 🟡 متوسط |
| 10  | 📱 **Deep Linking**        | روابط مباشرة للتطبيق        | 🟡 متوسط |

### واجهة الويب (Web Frontend) — الحالة الحالية

| #   | الصفحة/الميزة             | الوصف                    | الأولوية |
| --- | ------------------------- | ------------------------ | -------- |
| 1   | 🏠 **Landing Page**       | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 2   | 🍴 **Restaurant List**    | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 3   | 📋 **Restaurant Menu**    | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 4   | 🛒 **Shopping Cart**      | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 5   | 💳 **Checkout**           | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 6   | 📍 **Order Tracking**     | ✅ تم تنفيذها (MVP)      | 🔴 حرج   |
| 7   | 📜 **Order History**      | ✅ تم تنفيذها (MVP)      | 🟠 عالي  |
| 8   | 👤 **User Profile**       | ✅ تم تنفيذها (MVP)      | 🟠 عالي  |
| 9   | 🏪 **Provider Dashboard** | ✅ تم تنفيذها (MVP)      | 🟠 عالي  |
| 10  | 🚗 **Driver Dashboard**   | 🟡 صفحات placeholder فقط | 🟠 عالي  |

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

## 🆕 آخر التحديثات (v0.0.13)

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

### تحديثات إضافية بعد v0.0.13 (حتى 15 فبراير 2026)

#### Backend

| التحديث                           | الوصف                                                                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| **Phone OTP Channel Routing**     | إرسال كود تحقق الهاتف عبر **SMS** أو **WhatsApp** حسب اختيار المستخدم (`channel=sms                                       | whatsapp`) مع قوالب رسائل احترافية. |
| **Twilio SMS/WhatsApp Hardening** | تحسين قراءة متغيرات البيئة (منع مشاكل cwd) + تحسين رسائل الخطأ (خصوصاً Geo Permissions و WhatsApp sender غير مُفعّل).     |
| **Sender Branding Options**       | دعم استخدام Twilio **Messaging Service SID** و/أو Sender ID (حسب البلد/المشغل) لإظهار اسم مرسل مناسب بدل رقم عند الإمكان. |
| **Auth/Register Robustness**      | معالجة تعارضات التسجيل/الدخول الناتجة عن بيانات محذوفة سابقاً + تحويل تعارض الـ unique إلى 409 بشكل واضح.                 |
| **API Prefix From ENV**           | جعل `API_PREFIX` من `.env` هو المصدر الفعلي للـ global prefix بدل hardcode.                                               |
| **Stripe Webhook Receiver**       | إضافة Webhook endpoint للتحقق من `stripe-signature` باستخدام raw body.                                                    |

**Stripe webhook endpoint (Production):**

- `POST https://<DOMAIN>/{API_PREFIX}/payments/stripe/webhook`
- مثال شائع: `POST https://<DOMAIN>/api/v1/payments/stripe/webhook`

#### Web Frontend

| التحديث                  | الوصف                                                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Phone Verify UX**      | عند تحقق رقم الهاتف يظهر خيارين: SMS أو WhatsApp، والواجهات/الـ toasts تم توحيدها لتكون channel-agnostic.                                                     |
| **Add Card UX (Stripe)** | بدل إعادة توجيه المستخدم لصفحة Stripe Hosted، تم اعتماد **Stripe Payment Element داخل الموقع** بتصميم مطابق للثيم واستخدام نفس لوجو الفافيكون (`/LOGOM.png`). |

#### تحديثات إضافية (Store + Admin)

| التحديث                                    | الوصف                                                                                                     |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| **Admin Promotions (Restaurants scope)**   | إنشاء promo code وربطه بمطاعم محددة عبر `restaurantIds` + تطبيق التحقق/الخصم في الـ checkout حسب المطعم.  |
| **Promo Validity (Start/End)**             | دعم `startsAt/endsAt` في إنشاء البرومو، وعرضها في الأدمن، مع فلترة “شغال ولسا مش منتهي”.                  |
| **Promo Notifications (English defaults)** | الافتراضي لإشعار البرومو أصبح بالإنجليزي (مع إمكانية تخصيص title/body من الأدمن).                         |
| **Admin Promotions Active List UI**        | إضافة جدول “Active promotions” في صفحة الأدمن يعرض البروموهات الفعالة غير المنتهية + حالات loading/empty. |
| **Auth Token Scoping Fix**                 | منع إرسال admin token على مسارات المستخدم لتجنب Unauthorized في صفحات المطاعم (خصوصاً cart preload).      |
| **Cart Invoice Currency**                  | عرض إجمالي/فاتورة الكارت باليورو EUR باستخدام `Intl.NumberFormat`.                                        |
| **Home Redirect**                          | جعل `/` يفتح صفحة المطاعم `/restaurants` افتراضياً، مع إبقاء صفحة الهبوط متاحة على `/home`.               |

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

| الميزة                          | الوصف                                                                                                          |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **HTTP Interceptors**           | Interceptors موحدة (Auth + Retry + Loading + Errors) على مستوى التطبيق                                         |
| **Global Loading State**        | Overlay عام يظهر أثناء أي HTTP request عبر LoadingService                                                      |
| **Unified Error Handling**      | ErrorInterceptor + GlobalErrorHandler مع Toast موحد للأخطاء                                                    |
| **Landing Images Admin**        | صفحة الأدمن أصبحت لإدارة صور اللاندنج (5 Slots)                                                                |
| **About Images Admin**          | صفحة أدمن جديدة لإدارة صور صفحة About (5 Slots)                                                                |
| **Landing Page Dynamic Hero**   | الهيرو سيكشن يدعم صورة خلفية ديناميكية مع نفس التقوس                                                           |
| **Drag & Drop Upload**          | رفع الصور بالسحب والإفلات أو الضغط للتصفح                                                                      |
| **Preview with Curve**          | معاينة الصورة مع نفس شكل التقوس السفلي                                                                         |
| **7 Languages Support**         | صفحة الهبوط تدعم 7 لغات (en, fr, lb, de, it, pt, es)                                                           |
| **WebSocket Real-time**         | تحديث فوري لصور اللاندنج (Hero + Why) باستخدام Socket.IO                                                       |
| **Notifications UI (MVP)**      | صفحة `/notifications` + badge في navbar + Mark as read/all + realtime                                          |
| **Orders UI (MVP)**             | داخل Profile: Order history + زر Track يفتح `/orders/:id/tracking` بخريطة                                      |
| **Web Console Warning Fix**     | إصلاح تحذير DevTools (إضافة `id/name` لحقل البحث في Landing)                                                   |
| **Perf Baseline (local)**       | LCP ~ 896ms، CLS ~ 0.23 (Needs improvement)                                                                    |
| **Cute Navbar Tabs**            | أزرار الـ Cute Navbar تعمل كتبديل لعرض الصور والنصوص                                                           |
| **Skeleton + Toast Loading**    | Skeleton + Lazy loading للصور + Loading toast عند البطء                                                        |
| **About Page UI**               | صفحة About جديدة بألوان Landing v2 + تداخل سيكشنات                                                             |
| **Restaurants Listing (MVP)**   | قائمة مطاعم حقيقية من الـ API + بحث + فلتر Open Only                                                           |
| **Restaurant Details (MVP)**    | تبويبات التصنيفات + عرض الـ menu + Add to cart                                                                 |
| **Cart + Checkout (MVP)**       | تعديل الكميات + promo validate + Checkout                                                                      |
| **Navbar Search/Cart Badge**    | البحث يفتح restaurants مع query + عداد السلة                                                                   |
| **Theme Fix (User Pages)**      | إصلاح الدارك مود ليشمل الصفحات (Profile/Legal/About/…)                                                         |
| **Theme RGB Helpers**           | إضافة `--ink-rgb`/`--paper-rgb` وتطبيقها في CSS overlays                                                       |
| **Placeholder Pages Themed**    | صفحات Driver/Provider أصبحت تستخدم ألوان الثيم                                                                 |
| **User Tokens Flow**            | تسجيل دخول المستخدم أصبح عبر `/auth/user/login` + تخزين access/refresh tokens                                  |
| **User Logout Revocation**      | logout يعمل best-effort على `/auth/user/logout` ثم يمسح session + tokens                                       |
| **Partner Places Admin**        | صفحة `/admin/partner-places` لإدارة الطلبات + Actions (Approve/Details/Reject/Message/Delete)                  |
| **Partner Places Real-time**    | تحديث لحظي لجدول Partner Places عبر Socket.IO (listen `partner-places.changed` ثم reload)                      |
| **Delete Provider Master Code** | عند حذف Provider/Request: يظهر Modal لكتابة Master Code + عرض Attempts المتبقية + Lock لمدة دقائق مع Countdown |
| **Provider Portal UX**          | منع تسجيل Provider أثناء وجود جلسة User + رسالة إنجليزية ضمن نظام ترجمة 7 لغات                                 |
| **Provider Navbar Label**       | شارة “Provider” تظهر في navbar عند وجود جلسة Provider                                                          |

### Technical Details

```
Backend:
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

---

## 📞 الخطوات القادمة

1. **فوراً**: ضبط thresholds الخاصة بـ Unusual Activity في بيئة التطوير لتقليل false positives (env المذكورة أعلاه)
2. **هذا الأسبوع**: إضافة اختبارات e2e بسيطة لمسارات Orders (me/tracking) + Notifications (read/readAll)
3. **التالي مباشرة**: Drivers/Delivery Zones + Real-time tracking (خريطة)
4. **تحسين سريع**: تحسين observability للـ bans (تجميع counters في Monitoring Logs stats)
5. **اختياري لاحقاً**: جعل bans/attempts persistent (Redis/DB) بدل in-memory لو احتجنا سلوك ثابت بعد restart

---

<div align="center">

**📝 آخر تحديث: 15 فبراير 2026**

[![Made with ❤️](https://img.shields.io/badge/Made%20with-❤️-red.svg)]()
[![For Moien Delivery](https://img.shields.io/badge/For-Moien%20Delivery-blue.svg)]()

</div>
