<div align="center">

# 🍽️ Moien Delivery | موين دليفري

### منصة توصيل الطعام الذكية | Smart Food Delivery Platform

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![NestJS](https://img.shields.io/badge/NestJS-11.0-ea2845.svg)](https://nestjs.com/)
[![Angular](https://img.shields.io/badge/Angular-21.0-dd0031.svg)](https://angular.io/)
[![Flutter](https://img.shields.io/badge/Flutter-3.10-02569B.svg)](https://flutter.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178c6.svg)](https://www.typescriptlang.org/)
[![Dart](https://img.shields.io/badge/Dart-3.10-0175C2.svg)](https://dart.dev/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/moien-delivery/moien-delivery?style=social)](https://github.com/moien-delivery/moien-delivery)

<p align="center">
  <img src="docs/assets/logo.png" alt="Moien Delivery Logo" width="200"/>
</p>

**🌍 متوفر باللغة العربية | 🇱🇺 Verfügbar op Lëtzebuergesch**

[العربية](#العربية) • [Lëtzebuergesch](#lëtzebuergesch) • [Documentation](docs/) • [API Reference](docs/API.md)

---

<p align="center">
  <img src="docs/assets/hero-banner.png" alt="Moien Delivery Banner" width="100%"/>
</p>

</div>

---

## العربية

### 📋 نظرة عامة

**موين دليفري** هي منصة متكاملة لتوصيل الطعام مستوحاة من أفضل التطبيقات العالمية مثل **Uber Eats** و **Wolt** و **طلبات** و **Deliveroo**. تقدم المنصة تجربة سلسة للمستخدمين والمطاعم وسائقي التوصيل من خلال نظام بيئي متكامل يشمل:

- 🌐 **موقع ويب** - صفحة رئيسية جذابة + لوحة تحكم إدارية متكاملة
- 📱 **تطبيق العملاء والمطاعم** - تطبيق موحد للطلب وإدارة المطاعم
- 🚗 **تطبيق السائقين** - تطبيق مخصص لسائقي التوصيل

---

### ✨ المميزات الرئيسية

<table>
<tr>
<td width="50%">

#### 👤 للمستخدمين

- 🔍 البحث الذكي عن المطاعم والأطباق
- 📍 تحديد الموقع الجغرافي بدقة عالية
- 🛒 سلة تسوق متعددة المطاعم
- 💳 طرق دفع متنوعة ( بطاقات)
- ⏱️ تتبع الطلب مباشرة على الخريطة
- ⭐ نظام التقييمات والمراجعات
- 🎁 برنامج الولاء والمكافآت
- 📜 سجل الطلبات وإعادة الطلب بضغطة واحدة
- 💬 دردشة مباشرة مع السائق والمطعم
- 🔔 إشعارات فورية لحالة الطلب

</td>
<td width="50%">

#### 🍴 للمطاعم

- 📊 لوحة تحكم شاملة للإحصائيات
- 📝 إدارة القوائم والأصناف بسهولة
- ⏰ إدارة أوقات العمل والتوفر
- 📸 رفع صور احترافية للأطباق
- 💰 تقارير مالية مفصلة
- 📈 تحليلات المبيعات والأداء
- 🏷️ إدارة العروض والخصومات
- 🔔 إشعارات الطلبات الجديدة
- ⚡ قبول/رفض الطلبات بسرعة
- 📱 تطبيق موبايل لإدارة الطلبات

</td>
</tr>
<tr>
<td width="50%">

#### 🚴 للسائقين

- 🗺️ نظام ملاحة متكامل
- 💵 حساب الأرباح اليومية والأسبوعية
- 📊 إحصائيات الأداء الشخصي
- 🔄 اختيار الطلبات المناسبة
- 💰 نظام البقشيش الإلكتروني
- 📍 تحديد مناطق العمل المفضلة
- ⏱️ جدولة أوقات العمل
- 🏆 نظام التصنيف والمكافآت
- 📞 التواصل مع العملاء والدعم
- 🚨 زر الطوارئ للسلامة

</td>
<td width="50%">

#### 🔧 لوحة التحكم الإدارية

- 👥 إدارة المستخدمين والصلاحيات
- 🏪 إدارة المطاعم والموافقات
- 🚗 إدارة السائقين والتحقق
- 📊 تقارير شاملة ومفصلة
- 💳 إدارة المدفوعات والتسويات
- 🎫 نظام الكوبونات والعروض
- 📍 إدارة المناطق والتغطية
- 🔔 نظام الإشعارات الجماعية
- 📈 لوحة معلومات تفاعلية
- ⚙️ إعدادات النظام المتقدمة

</td>
</tr>
</table>

---

### 🏗️ هيكل المشروع

```
moien-delivery/
├── 📁 backend/                 # الخادم الخلفي (NestJS)
│   ├── src/
│   │   ├── modules/           # الوحدات (المستخدمين، الطلبات، المطاعم...)
│   │   ├── common/            # الأدوات المشتركة
│   │   ├── config/            # إعدادات التطبيق
│   │   └── database/          # قاعدة البيانات
│   └── test/                  # الاختبارات
│
├── 📁 wfrontend/               # واجهة الويب (Angular)
│   ├── src/
│   │   ├── app/
│   │   │   ├── landing/       # الصفحة الرئيسية
│   │   │   ├── admin/         # لوحة التحكم الإدارية
│   │   │   └── shared/        # المكونات المشتركة
│   │   └── assets/            # الملفات الثابتة
│   └── public/                # الملفات العامة
│
├── 📁 mfrontend/               # تطبيقات الموبايل (Flutter)
│   ├── lib/
│   │   ├── customer/          # تطبيق العملاء
│   │   ├── restaurant/        # تطبيق المطاعم
│   │   ├── driver/            # تطبيق السائقين
│   │   ├── core/              # النواة المشتركة
│   │   └── shared/            # المكونات المشتركة
│   └── test/                  # الاختبارات
│
└── 📁 docs/                    # التوثيق
    ├── API.md                 # توثيق API
    ├── SETUP.md               # دليل الإعداد
    ├── DEPLOYMENT.md          # دليل النشر
    └── CONTRIBUTING.md        # دليل المساهمة
```

---

### 🛠️ التقنيات المستخدمة

<table>
<tr>
<td align="center" width="20%">

**Backend**

<img src="https://nestjs.com/img/logo-small.svg" width="50" height="50"/>

NestJS 11

</td>
<td align="center" width="20%">

**Web**

<img src="https://angular.io/assets/images/logos/angular/angular.svg" width="50" height="50"/>

Angular 21

</td>
<td align="center" width="20%">

**Mobile**

<img src="https://storage.googleapis.com/cms-storage-bucket/0dbfcc7a59cd1cf16282.png" width="50" height="50"/>

Flutter 3.10

</td>
<td align="center" width="20%">

**Database**

<img src="https://www.postgresql.org/media/img/about/press/elephant.png" width="50" height="50"/>

PostgreSQL

</td>
<td align="center" width="20%">

**Cache**

<img src="https://redis.io/images/redis-white.png" width="50" height="50"/>

Redis

</td>
</tr>
</table>

#### المزيد من التقنيات:

| الفئة        | التقنيات                              |
| ------------ | ------------------------------------- |
| 🔐 المصادقة  | JWT, OAuth 2.0, Firebase Auth         |
| 🗺️ الخرائط   | Google Maps API, Mapbox               |
| 💳 المدفوعات | Stripe |
| 📨 الإشعارات | Firebase Cloud Messaging, OneSignal   |
| 📊 التحليلات | Google Analytics, Mixpanel            |
| 🔍 البحث     | Elasticsearch, Algolia                |
| 📁 التخزين   | AWS S3, Cloudinary                    |
| 🚀 النشر     | Docker, Kubernetes, AWS/GCP           |
| 🔄 CI/CD     | GitHub Actions, GitLab CI             |
| 📝 التوثيق   | Swagger/OpenAPI, Compodoc             |

---

### 🚀 البدء السريع

#### المتطلبات الأساسية

```bash
# Node.js 20+
node --version

# npm 10+
npm --version

# Flutter 3.10+
flutter --version

# PostgreSQL 15+
psql --version
```

#### التثبيت

```bash
# استنساخ المشروع
git clone https://github.com/moien-delivery/moien-delivery.git
cd moien-delivery

# تثبيت تبعيات الخادم
cd backend
npm install

# تثبيت تبعيات الويب
cd ../wfrontend
npm install

# تثبيت تبعيات الموبايل
cd ../mfrontend
flutter pub get
```

#### تشغيل المشروع

```bash
# تشغيل الخادم
cd backend
npm run start:dev

# تشغيل الويب (في terminal جديد)
cd wfrontend
npm start

# تشغيل تطبيق الموبايل (في terminal جديد)
cd mfrontend
flutter run
```

---

### 📱 لقطات الشاشة

<table>
<tr>
<td align="center">
<img src="docs/assets/screenshots/home.png" width="200"/>
<br><b>الصفحة الرئيسية</b>
</td>
<td align="center">
<img src="docs/assets/screenshots/restaurants.png" width="200"/>
<br><b>قائمة المطاعم</b>
</td>
<td align="center">
<img src="docs/assets/screenshots/order.png" width="200"/>
<br><b>تتبع الطلب</b>
</td>
<td align="center">
<img src="docs/assets/screenshots/admin.png" width="200"/>
<br><b>لوحة التحكم</b>
</td>
</tr>
</table>

---

### 📊 خريطة الطريق

- [x] 🎯 **المرحلة 1**: إنشاء البنية الأساسية
- [x] 🎯 **المرحلة 2**: نظام المصادقة والمستخدمين
- [ ] 🚧 **المرحلة 3**: إدارة المطاعم والقوائم
- [ ] 📋 **المرحلة 4**: نظام الطلبات والدفع
- [ ] 📋 **المرحلة 5**: تتبع الطلبات المباشر
- [ ] 📋 **المرحلة 6**: تطبيق السائقين
- [ ] 📋 **المرحلة 7**: لوحة التحكم الإدارية
- [ ] 📋 **المرحلة 8**: الإشعارات والتنبيهات
- [ ] 📋 **المرحلة 9**: نظام التقييمات
- [ ] 📋 **المرحلة 10**: الإطلاق التجريبي

---

### 🤝 المساهمة

نرحب بمساهماتكم! يرجى قراءة [دليل المساهمة](docs/CONTRIBUTING.md) للبدء.

```bash
# إنشاء فرع جديد
git checkout -b feature/amazing-feature

# حفظ التغييرات
git commit -m 'إضافة ميزة رائعة'

# رفع التغييرات
git push origin feature/amazing-feature
```

---

### 📄 الرخصة

هذا المشروع مرخص بموجب رخصة MIT - راجع ملف [LICENSE](LICENSE) للتفاصيل.

---

### 📞 التواصل

<p align="center">
  <a href="https://twitter.com/moiendelivery">
    <img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white" />
  </a>
  <a href="https://www.linkedin.com/company/moiendelivery">
    <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" />
  </a>
  <a href="mailto:contact@moiendelivery.com">
    <img src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" />
  </a>
  <a href="https://discord.gg/moiendelivery">
    <img src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white" />
  </a>
</p>

---

## Lëtzebuergesch

### 📋 Iwwersiicht

**Moien Delivery** ass eng komplett Liewermittelsplattform inspiréiert vun de beschten internationalen Uwendungen wéi **Uber Eats**, **Wolt**, **Talabat** a **Deliveroo**. D'Plattform bitt eng nahtlos Erfarung fir Benotzer, Restauranten a Liwwerer duerch en integréiert Ökosystem:

- 🌐 **Websäit** - Attraktiv Heemssäit + Komplett Administratiounspanel
- 📱 **Client & Restaurant App** - Vereenegt App fir Bestellungen an Restaurantmanagement
- 🚗 **Driver App** - Dedizéiert App fir Liwwerer

### ✨ Haaptfunktiounen

#### 👤 Fir Benotzer

- 🔍 Intelligent Sich no Restauranten an Wieder
- 📍 Präzis Geolokaliséierung
- 🛒 Multi-Restaurant Akeefswon
- 💳 Verschidden Bezuelmethoden
- ⏱️ Live Bestellverfolgung op der Kaart
- ⭐ Bewäertungen a Kritiken System

#### 🍴 Fir Restauranten

- 📊 Komplett Statistik Dashboard
- 📝 Einfach Menü Verwaltung
- 💰 Detailléiert Finanzberichten
- 📈 Verkaf an Performance Analyse

#### 🚴 Fir Liwwerer

- 🗺️ Integréiert Navigatiounssystem
- 💵 Deeglech a wöchentlech Verdéngscht Rechnung
- 🏆 Ranking a Belounung System

---

### 🚀 Schnell Start

```bash
# Projet klonen
git clone https://github.com/moien-delivery/moien-delivery.git
cd moien-delivery

# Server Ofhängegkeeten installéieren
cd backend && npm install

# Web Ofhängegkeeten installéieren
cd ../wfrontend && npm install

# Mobil Ofhängegkeeten installéieren
cd ../mfrontend && flutter pub get
```

---

<div align="center">

### ⭐ Wann Dir de Projet gär hutt, gitt eis e Stär!

**Gemaach mat ADUD TEAM zu Lëtzebuerg**

[🔝 Zréck no uewen](#-moien-delivery--موين-دليفري)

</div>



