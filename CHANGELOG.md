# 📝 سجل التغييرات | Changelog

جميع التغييرات الملحوظة في هذا المشروع موثقة في هذا الملف.

يتبع هذا المشروع [Semantic Versioning](https://semver.org/lang/ar/).

---

## [غير مُصدر] - Unreleased

### 🚀 مضاف

- **Multi-Restaurant Cart System (Backend + Web)**
  - نظام سلة متعددة المطاعم: المستخدم يستطيع إضافة عناصر من مطاعم مختلفة بسلات منفصلة
  - `GET /orders/me/carts` — endpoint جديد لجلب كل سلات المستخدم النشطة
  - `CartStateService` أُعيد كتابته بالكامل: `cartMap` signal (خريطة سلات حسب المطعم) بدل سلة واحدة
  - `allCarts` computed signal يُرشّح السلات الفارغة
  - `loadAllCarts()` يجلب كل السلات عند تحميل الصفحة
  - `addItem()` / `setItemQuantity()` / `loadCart()` تُخزّن السلة المُرجعة في الخريطة

- **Cart Sidebar Component (Web)**
  - مكون `CartSidebarComponent` عالمي (standalone) مُضاف في `user-layout`
  - زر FAB عائم (أيقونة حقيبة تسوق `fa-shopping-bag`) مع badge أحمر لعدد العناصر الكلي
  - Sidebar جانبي يعرض السلات مجمّعة حسب المطعم (لوجو + اسم المطعم كعنوان)
  - أزرار تحكم بالكمية (+ / −) لكل عنصر
  - زر Checkout لكل مطعم ينقل لصفحة السلة مع `?restaurantId=`
  - تصميم responsive: FAB 56px → 50px على ≤600px → 46px على ≤400px

- **Cart Restaurant Enrichment (Backend)**
  - `enrichCartWithRestaurant()` دالة مساعدة خاصة في `orders.service.ts`
  - تُرفق `restaurantName` و `restaurantLogo` لكل استجابة سلة
  - مُطبّقة على: `getOrCreateCart()`, `addItem()`, `updateCartItem()`
  - `listMyCarts()` تُثري السلات بشكل مجمّع عبر `rMap`

- **Menu Item Preview Modal (Web)**
  - زر معلومات (ℹ) على كل بطاقة عنصر منيو لمعاينة الصورة
  - مودال معاينة بقياس كامل مع صورة العنصر + اسم + وصف + سعر
  - دعم إيماءات اللمس: pinch-to-zoom, pan/drag بإصبع واحد, double-tap zoom (1x ↔ 2.5x)
  - دعم الماوس: تكبير عبر عجلة الماوس (±0.25 لكل خطوة، 1x–5x) + سحب للتحريك
  - `previewTransform` computed signal: `translate(panX, panY) scale(zoom)`
  - `_clampPan()` يحد التحريك بـ `±150 * (zoom - 1)` بكسل
  - نص تلميح "Pinch to zoom · Drag to pan"
  - breakpoints مستجيبة: 920px, 600px, 400px

- **Item Actions in Copy Area (Web)**
  - نقل أزرار Add + Info من فوق صورة العنصر إلى منطقة النص (`.rd-item-copy`)
  - حاوية `.rd-item-actions` بتخطيط flex مع gap: 8px
  - زر Info دائري 32px + زر Add على شكل pill بلون أساسي

- **Google Maps Migration (Web)**
  - استبدال Leaflet + OpenStreetMap بـ Google Maps JavaScript API في جميع الصفحات (6 ملفات)
  - الصفحات المُحدّثة: Order Tracking، Provider Dashboard، Provider Settings، Driver Dashboard، Location Modal، Partner Location Picker
  - تثبيت `@types/google.maps` + إضافة `places` و `marker` libraries
  - حذف حزم Leaflet (`leaflet`, `@types/leaflet`, `leaflet-routing-machine`) من المشروع
  - استخدام `AdvancedMarkerElement` بدل الـ markers التقليدية

- **Location Modal Redesign (Web)**
  - إعادة تصميم كامل لنافذة الموقع: بدل الكشف التلقائي، المستخدم يختار موقعه يدوياً على الخريطة
  - بحث أماكن عبر Google Places Autocomplete مع dropdown مُنسّق بالـ dark theme
  - الضغط على الخريطة يضع pin قابل للسحب + reverse geocode للعنوان
  - خيار Satellite/Hybrid لعرض القمر الصناعي مع أسماء المباني والمطاعم
  - أيقونة `fa-location-dot` للـ pin بدل الدائرة الزرقاء
  - زر "Confirm location" لحفظ الموقع المختار
  - `setManualLocation()` في `LocationService` للحفظ + reverse geocode + تحديث الكاش
  - ترجمة 5 مفاتيح جديدة في 7 لغات

- **PayPal Integration (Backend + Web)**
  - إضافة PayPal REST SDK (Sandbox + Live) كخيار دفع ثالث بجانب Stripe والدفع عند الاستلام
  - `POST /orders/paypal-create` — إنشاء PayPal order
  - `POST /orders/paypal-capture` — التقاط الدفع بعد موافقة المستخدم
  - زر PayPal في صفحة Checkout مع redirect flow كامل
  - تبديل تلقائي بين Sandbox و Live حسب `PAYPAL_MODE` في `.env`

- **Free Delivery for New Users (30 days)**
  - توصيل مجاني تلقائي لكل المستخدمين الجدد (خلال أول 30 يوم من التسجيل)
  - `isNewUserFreeDelivery()` helper في `orders.service.ts` مع تطبيق في 4 مسارات checkout
  - بانر أخضر "Free Delivery for New Members!" في صفحة تفاصيل المطعم
  - ترجمة البانر في 7 لغات
  - `createdAt` مضاف لـ `UserSession` type + متوفر في navbar login/signup/restore

- **Step-Based Infinite Marquee (Categories + Offers)**
  - تحويل الكاروسيل من CSS animation إلى JS `setInterval` + CSS `transition` (discrete steps + smooth easing)
  - كل خطوة تحرك عنصر واحد مع `cubic-bezier(0.22, 0.61, 0.36, 1)` ثم توقف مؤقت
  - التصنيفات: خطوة كل 2.2 ثانية لليمين، العروض: كل 3 ثوانٍ لليسار
  - إعادة ضبط صامتة عند اكتمال نصف الدورة (transition=none → reflow → restore)
  - إيقاف مؤقت عند hover

- **Marquee Arrows + Touch/Swipe Support**
  - أسهم يمين/يسار على كلا الشريطين (تظهر عند hover في الديسكتوب، ظاهرة دائماً على الموبايل)
  - دعم Touch/Swipe للموبايل (swipe يسار = تقدم، يمين = رجوع، عتبة 40px)
  - إيقاف مؤقت أثناء اللمس، استئناف تلقائي بعد 1.5 ثانية
  - حالة الماركي (step, itemAdvance, totalHalfPx, halfCount) كخصائص instance بدل متغيرات محلية
  - دعم التنقل العكسي (negative wrap-around)

- **About Page CTA Redesign**
  - تحويل بانر "Want to partner?" من زر واحد إلى زرين:
    - 🏪 "Register as Restaurant" → يوجه لصفحة `/provider` لتسجيل المطعم
    - 💬 "Apply as Driver" (أخضر واتساب) → يفتح واتساب برسالة جاهزة للتقديم كسائق
  - ترجمة الزرين في 7 لغات
  - تصميم responsive: الزرين يصيرون full-width على الموبايل

- **Category Images (Icons8 Color CDN)**
  - استبدال أيقونات Font Awesome بصور ملونة من Icons8 Color CDN لـ 20 تصنيف

- **Nginx Cache Fix for SPA**
  - إضافة `Cache-Control: no-cache, no-store, must-revalidate` لـ `index.html` في nginx config
  - حل مشكلة عدم ظهور التحديثات بعد الـ deploy

- **Phone OTP (SMS/WhatsApp via Twilio)**
  - دعم إرسال كود التحقق عبر `sms` أو `whatsapp` (مع hardening لرسائل أخطاء Twilio وملاحظات sandbox)

- **Stripe: Add Card + Webhook**
  - إضافة Stripe webhook receiver مع التحقق من التوقيع (raw body)
  - تحسين تجربة إضافة بطاقة (Add Card) في الويب عبر Stripe Setup Intent / Payment Element

- **Promotions (Admin) — Promo Codes**
  - CRUD كامل للعروض عبر `/promotions` للأدمن (JWT role=admin)
  - ربط العرض بمطاعم محددة + دعم `startsAt/endsAt` + فلترة `isActive`
  - إرسال إشعارات لمستخدمين محددين من العرض: `POST /promotions/:id/notify`

- **Promotional Offers v2 (Admin) — Visual Banners**
  - نظام عروض ترويجية مرئية (بانرات) منفصل عن أكواد الخصم
  - عناوين ووصف ثنائي اللغة (EN/AR) + صورة + نسبة خصم
  - نظام شروط مرن: `min_order` / `free_delivery` / `first_order` / `min_items`
  - ربط بمطاعم محددة + صلاحية زمنية (start/end)
  - إدارة كاملة (إنشاء/تعديل/حذف) من لوحة الأدمن

- **Restaurants Page Redesign (Web)**
  - عنوان "Our Providers" مع بحث فوري (instant search)
  - كاروسيل تصنيفات (Category Carousel) مع 20 تصنيف مُنسّق بألوان وأيقونات Font Awesome
  - كاروسيل عروض ترويجية (Offers Carousel) مع بطاقات بانر مرئية
  - أسهم تنقل (يمين/يسار) بألوان برتقالية (#e87e04) مع hover effect
  - تمرير تلقائي لا نهائي عبر `requestAnimationFrame` (تصنيفات: يمين، عروض: يسار)
  - تمرير بالسحب (drag-to-scroll) مع إيقاف مؤقت تلقائي (3-4 ثوانٍ)
  - حلقة لا نهائية (infinite loop) عبر تكرار العناصر (Set A + Set B) مع إعادة ضبط تلقائية
  - الضغط على بطاقة عرض ينقل المستخدم مباشرة إلى صفحة المطعم

- **Restaurant Detail Offer Banners (Web)**
  - عرض أفضل عرض ترويجي للمطعم الحالي كبانر في صفحة التفاصيل
  - حساب `bestOffer` تلقائياً عبر `computed()` signal

- **International Phone Calling Codes (Web)**
  - قائمة منسدلة لاختيار كود الاتصال الدولي عند التسجيل (داخل مودال Navbar)
  - دعم أكثر من 30 دولة مع أعلام
  - التحقق من عدد أرقام الهاتف حسب الدولة المختارة (digit count validation)

- **Auto-Login After Register (Web)**
  - تسجيل الدخول تلقائياً بعد التسجيل الناجح (بدون الحاجة لإدخال البيانات مرة أخرى)

- **Ratings Module (Backend)**
  - وحدة جديدة للتقييمات: `backend/src/modules/ratings/`
  - تقييم المطعم بعد الطلب: `GET /ratings/orders/:orderId/restaurant` + `POST /ratings/orders/:orderId/restaurant`
  - كيانات: `PlatformRatingEntity` (تقييم المنصة) + `RestaurantRatingEntity` (تقييم المطعم)
  - تقييمات المزودين: `PartnerRatingsController`

- **Partner Notifications Module (Backend)**
  - وحدة جديدة لإشعارات المزودين: `backend/src/modules/partner-notifications/`
  - إشعارات خاصة بالـ Provider عبر `PartnerJwtAuthGuard`
  - WebSocket Gateway لتحديثات لحظية للمزود
  - كيان `PartnerNotificationEntity`

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

- **Geocode Proxy (Backend)**
  - إضافة وحدة تحكم `GeocodeController` كبديل (proxy) لـ Nominatim لتجنب مشاكل CORS
  - `GET /geocode/reverse?lat=&lon=` — ترجمة عكسية (إحداثيات → عنوان)
  - `GET /geocode/search?q=` — بحث عن الموقع (عنوان → إحداثيات)
  - الواجهة الأمامية تستخدم البروكسي بدلاً من الاتصال المباشر بـ Nominatim

- **Stripe Customer Resilience (Backend)**
  - `getOrCreateCustomer()` في `users-billing.service.ts` و `orders.service.ts` يتحقق الآن من وجود العميل في Stripe قبل الاستخدام
  - إعادة إنشاء العميل تلقائياً إذا لم يعد موجوداً (حماية من الانتقال بين مفاتيح test/live)

- **Seed Demo Data (Backend)**
  - بيانات تجريبية شاملة: 25 مطعم + 5 متاجر + ~120 تصنيف + ~240 عنصر منيو + 6 عروض ترويجية + 1 كود خصم (WELCOME10)
  - `npm run db:seed` لإنشاء البيانات تلقائياً

- **Registration Simplification (Web)**
  - تبسيط عملية التسجيل: إزالة خطوة التحقق من الهاتف/البريد عند التسجيل
  - تسجيل فوري بعد إدخال البيانات الأساسية (الاسم، البريد، كلمة المرور)

- **Location Quick-Save (Web)**
  - زر حفظ سريع للموقع الجغرافي في صفحة العناوين مع دعم خريطة Leaflet

- **Open/Closed Badges (Web)**
  - شارات مرئية (Open/Closed) على بطاقات المطاعم تُعرض حسب ساعات العمل

- **Responsive Mobile Layout (Web)**
  - تحسين شامل للتصميم على الشاشات الصغيرة (الموبايل والتابلت)
  - أزرار footer ثابتة في الأسفل + تحسين session timeout UX

### 🔧 مُصلَح

- **Database Schema Fix (Backend)**
  - إضافة أعمدة `paymentMethod` و `paypalOrderId` لجدول الطلبات عبر `ALTER TABLE`

- **Backend Deploy Fix**
  - إصلاح مشكلة عدم تطبيق التغييرات بعد النشر (إعادة بناء `dist/` يدوياً + إعادة تشغيل PM2)
  - إصلاح أمر PM2 restart في سكربت النشر: `sudo -u moiendelivery-api bash -c "source nvm.sh && pm2 restart ..."`

- **Cart Sidebar Auto-Open Fix (Web)**
  - إزالة الفتح التلقائي للسلة الجانبية عند إضافة عنصر (إزالة `sidebarOpen.set(true)` من `addToCart()` و `confirmDiffRestaurant()`)

- **Dark Mode Category Text Fix (Web)**
  - إصلاح لون نص التصنيفات في الوضع الداكن ليكون مقروءاً

- **PayPal Sandbox→Live Fix (Backend)**
  - إصلاح مشكلة استخدام بيانات Sandbox في بيئة الإنتاج
  - تحديث المفاتيح إلى LIVE credentials

- **Nginx Stale File Deployment Fix**
  - إصلاح مشكلة أن nginx يخدم ملفات من root بدل `/browser/`
  - نسخ الملفات الجديدة إلى root directory حيث يخدمها nginx فعلياً

- **Leaflet Map Fix (Web)**
  - إصلاح خطأ `t.map is not a function` في Driver Dashboard بسبب عدم التعامل مع `.default` wrapper من esbuild
  - إضافة `((leafletModule as any).default ?? leafletModule)` لتوافق الاستيراد

- **Stripe "No such customer" Fix (Backend)**
  - إصلاح خطأ عند إضافة بطاقة دفع بسبب استخدام `stripeCustomerId` من وضع الاختبار مع مفاتيح الإنتاج
  - تصفية العملاء القدامى من قاعدة البيانات + إضافة التحقق التلقائي

- **Nominatim CORS Fix (Backend)**
  - إصلاح فشل تحديد الموقع الجغرافي بسبب حظر CORS من Nominatim عبر إنشاء geocode proxy في الخادم

- **Production Deploy Fixes**
  - إصلاح 404 على `/api/v1/promotional-offers/active` (stale dist build)
  - إصلاح مشكلة ملفات JavaScript القديمة في الإنتاج (stale chunks)
  - إصلاح صلاحيات ملفات `dist/` المملوكة لـ root

- **Error Handling (Web) — إصلاح شامل**
  - `ErrorInterceptor`: تجاهل أخطاء 401 (Unauthorized) و 403 (Forbidden) عالمياً — لم تعد تظهر كـ Toast للمستخدم
  - `GlobalErrorHandler`: تجاهل `HttpErrorResponse` و `"Unauthorized"` لمنع ظهور Toast مكرر
  - إضافة `X-Skip-Error-Toast` header للطلبات الاختيارية (promotional offers, cart preload, admin requests)

- **Auth (Web)**
  - إصلاح مشكلة تداخل توكن الأدمن مع مسارات المستخدم (حصر استخدام admin token على `/admin/*` فقط)

- **Theme/UI (Web)**
  - جعل الوضع الداكن هو الافتراضي عند أول تحميل (First paint) + تحسين تطبيق/تثبيت الثيم عبر `theme-*` classes و `data-theme`
  - إصلاح مشكلة أن `prefers-color-scheme: dark` قد يتجاوز اختيار المستخدم عند تفعيل Light theme
  - إزالة الـ gradients من صفحات المتجر الأساسية (Restaurants list/details) وربط الألوان بـ tokens الثيم (accent cyan)

- إصلاح CORS لطلبات صفحة `/` نحو `__access/*` عبر السماح بالـ same-origin (localhost/127.0.0.1 على نفس PORT)
- ضمان أن `__access/*` خارج الـ global prefix حتى تعمل من صفحة الجذر

- **Orders (Web + Backend)**
  - منع إتمام الطلب/الدفع فقط إذا المستخدم لا يمتلك أي عنوان (Address gating)
  - إضافة `deliveryAddress` في tracking + عرض عنوان التسليم في صفحة Track
  - توحيد عرض/استخدام العملة إلى EUR بدل EGP للطلبات/الكروت القديمة (normalization)

- **Testing (Backend)**
  - إصلاح DI في اختبار `AppController` عبر mock لـ `ServerAccessService` (tests أصبحت PASS)

### 🔄 قيد التطوير

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
