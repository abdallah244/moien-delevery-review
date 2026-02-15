# 📝 سجل التغييرات | Changelog

جميع التغييرات الملحوظة في هذا المشروع موثقة في هذا الملف.

يتبع هذا المشروع [Semantic Versioning](https://semver.org/lang/ar/).

---

## [غير مُصدر] - Unreleased

### 🚀 مضاف

- **Phone OTP (SMS/WhatsApp via Twilio)**
  - دعم إرسال كود التحقق عبر `sms` أو `whatsapp` (مع hardening لرسائل أخطاء Twilio وملاحظات sandbox)

- **Stripe: Add Card + Webhook**
  - إضافة Stripe webhook receiver مع التحقق من التوقيع (raw body)
  - تحسين تجربة إضافة بطاقة (Add Card) في الويب عبر Stripe Setup Intent / Payment Element

- **Promotions (Admin)**
  - CRUD كامل للعروض عبر `/promotions` للأدمن (JWT role=admin)
  - ربط العرض بمطاعم محددة + دعم `startsAt/endsAt` + فلترة `isActive`
  - إرسال إشعارات لمستخدمين محددين من العرض: `POST /promotions/:id/notify`

- **Notifications (User)**
  - إضافة عمليات مساعدة لإدارة الإشعارات: mark-all-read, delete-selected, clear-all-read

- **Web UX**
  - تحويل مسار `/` ليعيد التوجيه إلى `/restaurants` (واللاندنج على `/home`)
  - توحيد عرض إجمالي السلة/الفاتورة بعملة EUR

- إنشاء البنية الأساسية للمشروع
- إعداد الخادم الخلفي باستخدام NestJS 11
- إعداد واجهة الويب باستخدام Angular 21
- إعداد تطبيقات الموبايل باستخدام Flutter 3.10
- إنشاء التوثيق الشامل

- **Root Homepage Access Gate (Backend)**
  - إضافة Access modal gate على `/` بكود دخول
  - 3 محاولات خاطئة → حظر 5 دقائق مع countdown
  - DevTools detection (best-effort) → حظر 10 دقائق
  - Unusual activity monitoring → حظر ساعة
  - Endpoints: `GET /__access/status`, `POST /__access/check`, `POST /__access/devtools`

### 🔧 مُصلَح

- **Auth (Web)**
  - إصلاح مشكلة تداخل توكن الأدمن مع مسارات المستخدم (حصر استخدام admin token على `/admin/*` فقط)

- **Theme/UI (Web)**
  - جعل الوضع الداكن هو الافتراضي عند أول تحميل (First paint) + تحسين تطبيق/تثبيت الثيم عبر `theme-*` classes و `data-theme`
  - إصلاح مشكلة أن `prefers-color-scheme: dark` قد يتجاوز اختيار المستخدم عند تفعيل Light theme
  - إزالة الـ gradients من صفحات المتجر الأساسية (Restaurants list/details) وربط الألوان بـ tokens الثيم (accent cyan)

- إصلاح CORS لطلبات صفحة `/` نحو `__access/*` عبر السماح بالـ same-origin (localhost/127.0.0.1 على نفس PORT)
- ضمان أن `__access/*` خارج الـ global prefix حتى تعمل من صفحة الجذر

### 🔄 قيد التطوير

- نظام المطاعم
- نظام الطلبات
- تطبيق الموبايل

---

## [0.0.13] - 2026-02-07

### ⚡ الأداء (Backend)

- تفعيل CacheModule مع Redis (اختياري) + fallback in-memory
- إضافة caching فعلي لـ Restaurants/Categories/MenuItems مع invalidation آمن عبر namespace versions
- تفعيل HTTP response compression عبر `compression` middleware
- إضافة Cache-Control/ETag للـ `/uploads` مع max-age قابل للضبط

### 🔐 المصادقة (Backend)

- اعتماد Auth module: `POST /api/v1/auth/user/login|refresh|logout` + `GET /api/v1/auth/me`
- Sessions endpoints: `GET /api/v1/auth/*/sessions` + revoke

---

## [0.0.12] - 2026-02-07

### 🗄️ قاعدة البيانات

- إضافة TypeORM migrations + DataSource لتشغيلها عبر CLI
- إضافة scripts:
  - `db:migrate:run`, `db:migrate:show`, `db:migrate:revert`
  - `db:seed`
- إضافة indexes أساسية لتحسين الأداء
- تفعيل soft delete (`deletedAt`) للكيانات الأساسية + تحويل عمليات الحذف الرئيسية إلى `softDelete`

---

## [0.0.11] - 2026-02-07

### 🔐 أمان (Backend)

- تفعيل Rate limiting فعلي (ThrottlerGuard) + إعدادات عبر env
- تفعيل Helmet + CORS whitelist قابلة للضبط
- تحسين Global ValidationPipe (whitelist/forbid/transform)
- إضافة CSRF middleware (اختياري عبر `CSRF_ENABLED=true`)

---

## [0.0.10] - 2026-02-07

### 🔧 مُصلَح

- **Theme (Web)**
  - إصلاح مشكلة أن الدارك مود كان يؤثر على الـ Navbar/Footer فقط في بعض الصفحات
  - إضافة متغيرات مساعدة `--ink-rgb` و `--paper-rgb` لاستخدامها في `rgba(...)` للـ borders/overlays/skeletons
  - تحويل عدة صفحات (Profile/Legal/Restaurants/Restaurant Details/… إلخ) لاستخدام ألوان الثيم بدل ألوان ثابتة

---

## [0.0.9] - 2026-02-07

### 🚀 مضاف

- **Restaurants + Menu (Backend + Web MVP)**
  - CRUD للمطاعم + التصنيفات + عناصر المنيو + filtering

- **Orders/Cart + Checkout (Backend + Web MVP)**
  - Cart كـ Order + إضافة/تعديل عناصر السلة + Checkout
  - Promo validation + تطبيق الخصم أثناء Checkout

- **Promotions (Backend)**
  - Validate endpoint + Tracking لاستخدام الكوبونات

- **Notifications (Backend)**
  - تخزين إشعارات + realtime عبر Socket.IO ضمن namespace `/notifications`

### 🧾 توثيق

- تحديث [REPORT.md](REPORT.md) لعرض تقدم الـ MVP والـ endpoints

---

## [0.0.8] - 2026-02-06

### 🚀 مضاف

- **About Page (Web)**
  - صفحة About جديدة (Wolt-inspired) مع تقليل السكاشن
  - ألوان متناسقة مع Landing v2 + تحسينات Contrast و UI
  - تأثير “stacked sections” (تداخل الكروت) لإحساس scroll أكثر فزيائية

- **Site Settings: About Images (Backend + Web Admin)**
  - Endpoints جديدة لإدارة صور About (Slots مستقلة عن اللاندنج):
    - `GET /api/v1/site-settings/about-images`
    - `POST /api/v1/site-settings/about-images/:slot/upload`
    - `DELETE /api/v1/site-settings/about-images/:slot`
  - WebSocket events جديدة ضمن namespace `/site-settings`:
    - `about-image.updated`
    - `about-images.updated`
  - صفحة أدمن جديدة لإدارة صور About:
    - `/admin/about-images`

### 🧾 توثيق

- تحديث [docs/API.md](docs/API.md) لإضافة Site Settings endpoints (Landing/About)
- تحديث [docs/WEBSOCKETS.md](docs/WEBSOCKETS.md) لإضافة `/site-settings` events
- تحديث [REPORT.md](REPORT.md) ليعكس تغييرات v0.0.8

---

## [0.0.5] - 2026-02-05

### 🚀 مضاف

- **تحديث Dashboard الخادم**
  - إضافة جميع الـ Endpoints الجديدة في لوحة التحكم
  - صفحات حالة (Status Pages) لكل endpoint
  - عرض حالة Online/Offline لكل endpoint
  - عرض وقت الاستجابة (Response Time)
  - تصميم موحد لجميع صفحات الحالة

- **Admin Staff Module - وحدة إدارة الموظفين**
  | Endpoint | Method | الوصف |
  |----------|--------|-------|
  | `/api/v1/admin-staff` | GET | قائمة الموظفين |
  | `/api/v1/admin-staff` | POST | إنشاء موظف جديد + رفع صورة |
  | `/api/v1/admin-staff/login` | POST | تسجيل دخول الأدمن |
  | `/api/v1/admin-staff/:id` | DELETE | حذف موظف |

- **Users Module - وحدة المستخدمين**
  | Endpoint | Method | الوصف |
  |----------|--------|-------|
  | `/api/v1/users` | GET | قائمة المستخدمين |
  | `/api/v1/users/summary` | GET | ملخص المستخدمين |
  | `/api/v1/users/register` | POST | تسجيل مستخدم جديد |
  | `/api/v1/users/login` | POST | تسجيل دخول |
  | `/api/v1/users/:id` | PATCH | تحديث بيانات المستخدم |
  | `/api/v1/users/:id` | DELETE | حذف مستخدم (Admin) |
  | `/api/v1/users/:id/photo` | POST | رفع صورة المستخدم |
  | `/api/v1/users/:id/ban` | POST | حظر مستخدم |
  | `/api/v1/users/:id/unban` | POST | إلغاء حظر مستخدم |
  | `/api/v1/users/:id/delete` | POST | حذف الحساب (Self) |
  | `/api/v1/users/:id/email/verify/send` | POST | إرسال كود تحقق الإيميل |
  | `/api/v1/users/:id/email/verify/confirm` | POST | تأكيد كود تحقق الإيميل |
  | `/api/v1/users/:id/phone/verify/send` | POST | إرسال كود تحقق الهاتف |
  | `/api/v1/users/:id/phone/verify/confirm` | POST | تأكيد كود تحقق الهاتف |
  | `/api/v1/users/:id/payment-methods` | GET | قائمة طرق الدفع |
  | `/api/v1/users/:id/payment-methods/setup-intent` | POST | إنشاء Setup Intent |
  | `/api/v1/users/:id/payment-methods/attach` | POST | ربط طريقة دفع |
  | `/api/v1/users/:id/payment-methods/:pmId/default` | POST | تعيين طريقة دفع افتراضية |
  | `/api/v1/users/:id/payment-methods/:pmId` | DELETE | حذف طريقة دفع |
  | `/api/v1/users/addresses/meta` | GET | بيانات العناوين الوصفية |
  | `/api/v1/users/:id/addresses` | GET | قائمة العناوين |
  | `/api/v1/users/:id/addresses` | POST | إضافة عنوان |
  | `/api/v1/users/:id/addresses/:addressId/default` | POST | تعيين عنوان افتراضي |
  | `/api/v1/users/:id/addresses/:addressId` | DELETE | حذف عنوان |

- **Support Tickets Module - وحدة تذاكر الدعم**
  | Endpoint | Method | الوصف |
  |----------|--------|-------|
  | `/api/v1/support-tickets` | GET | قائمة التذاكر |
  | `/api/v1/support-tickets` | POST | إنشاء تذكرة |
  | `/api/v1/support-tickets/:id/open` | POST | فتح تذكرة |
  | `/api/v1/support-tickets/:id/reply` | POST | الرد على تذكرة |

- **Payments Module - وحدة المدفوعات**
  | Endpoint | Method | الوصف |
  |----------|--------|-------|
  | `/api/v1/payments/stripe/publishable-key` | GET | مفتاح Stripe العام |

### 🔧 محسّن

- تحديث رقم الإصدار إلى v0.0.5
- إضافة عداد Modules في لوحة التحكم (4 Active)
- إضافة ألوان DELETE و PATCH في الـ CSS

---

## [0.0.4] - 2026-02-02

### 🚀 مضاف

- **لوحة تحكم المسؤول (Admin Panel)**

  #### 🔐 صفحة تسجيل الدخول (Admin Login)
  - تصميم Split Screen احترافي
  - قسم برتقالي يشرح مميزات لوحة التحكم
  - فورم تسجيل دخول بسيط (Email + Password)
  - دعم اللغة العربية والإنجليزية
  - Toast notifications للرسائل
  - Rate Limiting للحماية من هجمات Brute Force
  - حفظ الجلسة في localStorage

  #### 📊 لوحة التحكم (Admin Dashboard)
  - Admin Navbar مخصص مع روابط التنقل
  - Admin Footer مخصص
  - إحصائيات سريعة (Restaurants, Orders, Drivers, Users)
  - تصميم Responsive
  - دعم كامل للـ Themes

  #### 🎨 نظام الثيمات المحسن
  - إصلاح CSS Variables لتعمل مع Angular View Encapsulation
  - استخدام `ViewEncapsulation.None` للـ components
  - قيم مباشرة للألوان بدلاً من variables متداخلة
  - تطبيق فوري عند تغيير الثيم

  #### 🏗️ نظام Layouts
  - User Layout: يحتوي على Navbar و Footer للمستخدمين
  - Admin Layout: بدون Navbar و Footer العادية
  - فصل كامل بين صفحات المستخدم والمسؤول

### 🔧 محسّن

- تحسين CSS Variables للعمل مع كل الـ themes
- إضافة `html.theme-*` selectors للتوافق الكامل

### 📦 التقنيات والمكتبات

- Angular Guards للحماية
- ViewEncapsulation.None للـ CSS Variables
- localStorage للجلسات

---

## [0.0.3] - 2026-02-02

### 🚀 مضاف

- **واجهة الويب (Angular Frontend)**

  #### 🧭 شريط التنقل (Navbar)
  - تصميم احترافي مشابه لـ Wolt/Uber Eats
  - لوجو المنصة على اليسار
  - محدد الموقع (Location Selector)
  - خانة بحث متوسعة مع Animation سلسة
  - أزرار تسجيل الدخول والتسجيل
  - دعم كامل للـ CSS Variables والـ Themes

  #### 🦶 التذييل (Footer)
  - تصميم متعدد الأعمدة مع أقسام منظمة
  - روابط App Store و Google Play
  - أقسام: Partner, Company, Products, Support, Follow Us
  - أزرار اختيار الموقع واللغة والثيم
  - دعم Font Awesome للأيقونات

  #### 🌍 نظام الترجمة متعدد اللغات (i18n System)
  - **7 لغات مدعومة:**
    - 🇬🇧 English
    - 🇫🇷 Français (French)
    - 🇱🇺 Lëtzebuergesch (Luxembourgish)
    - 🇩🇪 Deutsch (German)
    - 🇮🇹 Italiano (Italian)
    - 🇵🇹 Português (Portuguese)
    - 🇪🇸 Español (Spanish)
  - Language Service مركزي لإدارة الترجمات
  - Language Modal لاختيار اللغة
  - حفظ اللغة في localStorage
  - ترجمة فورية بدون إعادة تحميل الصفحة

  #### 🎨 نظام الثيمات (Theme System)
  - **4 ثيمات متاحة:**
    - 🌗 Auto - يتبع إعدادات النظام تلقائياً
    - ☀️ Light - وضع فاتح
    - 🌙 Dark - وضع داكن
    - 👁️ High Contrast - تباين عالي للوصولية
  - Theme Service لإدارة الثيمات
  - Theme Modal احترافي مع أيقونات Font Awesome
  - CSS Variables لكل الألوان
  - حفظ الثيم في localStorage
  - تطبيق فوري على كل المكونات

### 📦 التقنيات والمكتبات

- Font Awesome 6.5.1 (CDN)
- CSS Variables للثيمات
- Angular Signals للـ State Management
- localStorage للحفظ المحلي

---

## [0.0.2] - 2026-02-01

### 🚀 مضاف

- **22 خدمة خلفية متكاملة (Backend Services)**

  #### 🔐 خدمات الأمان (Security Services) - 5 خدمات
  - `RateLimiterService` - حماية من DDoS والـ Brute Force مع خوارزمية Sliding Window
  - `EncryptionService` - تشفير AES-256-GCM، هاش كلمات المرور PBKDF2، توليد OTP
  - `JwtAuthService` - إدارة Access/Refresh Tokens، القائمة السوداء، التحقق
  - `IpGuardService` - القوائم البيضاء/السوداء، كشف الأنماط المشبوهة
  - `SanitizerService` - منع XSS وSQL Injection، تنظيف HTML

  #### ⚡ خدمات الأداء (Performance Services) - 5 خدمات
  - `CacheService` - تخزين مؤقت عالي الأداء مع TTL
  - `CompressionService` - ضغط Gzip و Brotli للاستجابات
  - `QueryOptimizerService` - تحليل وتحسين استعلامات قاعدة البيانات
  - `ConnectionPoolService` - إدارة ومراقبة اتصالات قاعدة البيانات
  - `ResponseOptimizerService` - مراقبة وتحسين أوقات الاستجابة

  #### 🔄 خدمات الوقت الحقيقي (Real-time Services) - 1 خدمة
  - `WebSocketService` - اتصالات في الوقت الحقيقي للطلبات والتتبع والدردشة

  #### 🎨 خدمات واجهة المستخدم (UI Services) - 3 خدمات
  - `LoadingService` - إدارة حالات التحميل (Skeleton, Spinner, Shimmer)
  - `ToastService` - إشعارات Toast مع أنماط متعددة
  - `ValidationService` - التحقق الشامل من المدخلات

  #### 📧 خدمات التواصل (Communication Services) - 3 خدمات
  - `EmailService` - إرسال البريد الإلكتروني مع قوالب HTML
  - `SmsService` - إرسال الرسائل القصيرة وأكواد OTP
  - `NotificationService` - إشعارات الـ Push والـ In-App

  #### 🛠️ خدمات إضافية (Utility Services) - 5 خدمات
  - `FileUploadService` - رفع الملفات والصور مع التحقق
  - `LoggerService` - تسجيل متقدم مع بحث وتحليلات
  - `GeolocationService` - حساب المسافات ومناطق التوصيل
  - `SearchService` - بحث نصي كامل في المطاعم والأطباق
  - `AnalyticsService` - تتبع سلوك المستخدم وتحليلات الأعمال
  - `QueueService` - معالجة المهام في الخلفية
  - `BackupService` - نسخ احتياطي واستعادة قاعدة البيانات

### 📦 المكتبات المثبتة

- @nestjs/throttler (Rate Limiting)
- @nestjs/jwt (JWT Authentication)
- @nestjs/passport (Passport Integration)
- passport-jwt (JWT Strategy)
- helmet (Security Headers)
- express-rate-limit (Rate Limiting)
- compression (Response Compression)
- ioredis (Redis Client)
- @nestjs/cache-manager (Caching)
- cache-manager-ioredis-yet (Redis Cache)
- @nestjs/websockets (WebSocket Support)
- @nestjs/platform-socket.io (Socket.IO)
- socket.io (Real-time Communication)
- @nestjs/event-emitter (Event System)
- nodemailer (Email)
- @nestjs/bull (Queue System)
- bull (Background Jobs)
- uuid (Unique IDs)
- multer (File Upload)
- @nestjs/schedule (Scheduled Tasks)
- sanitize-html (HTML Sanitization)
- crypto-js (Encryption)

---

## [0.0.1] - 2026-02-01

### 🚀 مضاف

- **إعداد قاعدة البيانات PostgreSQL**
  - تثبيت TypeORM و pg driver
  - إنشاء ملفات التكوين (database.config.ts, app.config.ts, jwt.config.ts)
  - دعم متغيرات البيئة عبر @nestjs/config

- **صفحة حالة الخادم (Server Dashboard)**
  - تصميم بسيط وأنيق بألوان فحمية وأبيض/رمادي
  - عرض معلومات الخادم (الإصدار، البيئة، المنفذ، الوقت)
  - مؤشر حالة الاتصال (أخضر للنجاح، أحمر للفشل)
  - أنيميشن بسيط وسلس

- **نظام حالة Endpoints**
  - صفحة حالة لكل endpoint بنفس التصميم
  - عرض حالة الـ endpoint (Online/Offline)
  - عرض وقت الاستجابة (Response Time)
  - زر العودة للوحة التحكم

- **ملفات البيئة**
  - إنشاء .env و .env.example
  - دعم إعدادات قاعدة البيانات والـ JWT والـ Redis والبريد

- **Endpoints متاحة**
  - `GET /` - لوحة تحكم الخادم
  - `GET /api/v1/health` - فحص صحة الخادم (JSON)
  - `GET /api/v1/health/status` - صفحة حالة Health endpoint
  - `GET /api/docs` - صفحة حالة التوثيق

### 📦 المكتبات المثبتة

- @nestjs/typeorm
- typeorm
- pg
- @nestjs/config
- class-validator
- class-transformer
- bcrypt
- @types/bcrypt

---

## [0.1.0] - 2026-02-01 (مخطط)

### 🚀 مضاف

- الإصدار الأولي للمشروع
- هيكل المشروع الأساسي
- إعداد بيئة التطوير
- التوثيق الأولي

---

## الأسطورة

- 🚀 **مضاف** - للميزات الجديدة
- 🔄 **تغيير** - للتغييرات في الوظائف الحالية
- ⚠️ **مهمل** - للميزات التي ستتم إزالتها قريباً
- 🗑️ **محذوف** - للميزات المحذوفة
- 🐛 **إصلاح** - لإصلاح الأخطاء
- 🔒 **أمان** - في حالة الثغرات الأمنية
