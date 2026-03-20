# 🤝 دليل المساهمة | Contributing Guide

<div align="center">

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)]()
[![Contributors](https://img.shields.io/badge/Contributors-Welcome-orange.svg)]()

**شكراً لاهتمامك بالمساهمة في Moien Delivery! 🎉**

</div>

---

## 📋 جدول المحتويات

- [قواعد السلوك](#-قواعد-السلوك)
- [كيف يمكنني المساهمة؟](#-كيف-يمكنني-المساهمة)
- [إعداد بيئة التطوير](#-إعداد-بيئة-التطوير)
- [معايير الكود](#-معايير-الكود)
- [عملية Pull Request](#-عملية-pull-request)
- [الإبلاغ عن الأخطاء](#-الإبلاغ-عن-الأخطاء)
- [اقتراح الميزات](#-اقتراح-الميزات)

---

## 📜 قواعد السلوك

### تعهدنا

نحن ملتزمون بتوفير بيئة ودية وآمنة للجميع. نتوقع من جميع المساهمين:

- ✅ استخدام لغة مرحبة وشاملة
- ✅ احترام وجهات النظر المختلفة
- ✅ قبول النقد البناء بلطف
- ✅ التركيز على ما هو أفضل للمجتمع
- ✅ إظهار التعاطف مع الأعضاء الآخرين

### غير مقبول

- ❌ التحرش بأي شكل
- ❌ التعليقات المهينة أو التمييزية
- ❌ نشر معلومات خاصة للآخرين
- ❌ أي سلوك غير مهني

---

## 🚀 كيف يمكنني المساهمة؟

### أنواع المساهمات

| النوع            | الوصف                  |
| ---------------- | ---------------------- |
| 🐛 إصلاح الأخطاء | إصلاح المشاكل الموجودة |
| ✨ ميزات جديدة   | إضافة وظائف جديدة      |
| 📚 التوثيق       | تحسين الوثائق          |
| 🧪 الاختبارات    | إضافة اختبارات         |
| 🎨 التصميم       | تحسين واجهة المستخدم   |
| 🌐 الترجمة       | ترجمة للغات جديدة      |
| ⚡ الأداء        | تحسين الأداء           |

### للمبتدئين

ابحث عن Issues بهذه العلامات:

- `good first issue` - مناسبة للمبتدئين
- `help wanted` - نحتاج مساعدة
- `documentation` - تحسين التوثيق

---

## 💻 إعداد بيئة التطوير

### المتطلبات

```bash
# Node.js 20+
node --version  # v20.0.0+

# npm 10+
npm --version   # 10.0.0+

# Flutter 3.10+
flutter --version  # 3.10.0+

# PostgreSQL 15+
psql --version  # 15.0+

# Docker (اختياري)
docker --version
```

### خطوات الإعداد

```bash
# 1. Fork المشروع على GitHub

# 2. استنساخ المشروع
git clone https://github.com/YOUR_USERNAME/moien-delivery.git
cd moien-delivery

# 3. إضافة upstream
git remote add upstream https://github.com/moien-delivery/moien-delivery.git

# 4. إنشاء فرع جديد
git checkout -b feature/your-feature-name

# 5. إعداد الخادم
cd backend
cp .env.example .env
npm install
npm run start:dev

# 6. إعداد الويب
cd ../wfrontend
npm install
npm start

# 7. إعداد الموبايل
cd ../mfrontend
flutter pub get
flutter run
```

### إعداد قاعدة البيانات

```bash
# باستخدام Docker
docker-compose up -d postgres redis

# أو يدوياً
createdb moien_delivery
npm run db:migrate:run
npm run db:seed
```

---

## 📏 معايير الكود

### TypeScript/JavaScript

```typescript
// ✅ صحيح
class OrderService {
  async createOrder(dto: CreateOrderDto): Promise<Order> {
    const order = this.orderRepository.create(dto);
    return this.orderRepository.save(order);
  }
}

// ❌ خاطئ
class order_service {
  async CreateOrder(DTO) {
    var order = this.orderRepository.create(DTO);
    return this.orderRepository.save(order);
  }
}
```

### Dart/Flutter

```dart
// ✅ صحيح
class OrderBloc extends Bloc<OrderEvent, OrderState> {
  final OrderRepository _orderRepository;

  OrderBloc(this._orderRepository) : super(OrderInitial()) {
    on<CreateOrder>(_onCreateOrder);
  }
}

// ❌ خاطئ
class orderBloc extends Bloc<OrderEvent, OrderState> {
  var orderRepository;

  orderBloc(this.orderRepository) : super(OrderInitial());
}
```

### قواعد التسمية

| النوع           | الأسلوب     | مثال               |
| --------------- | ----------- | ------------------ |
| Classes         | PascalCase  | `OrderService`     |
| Methods         | camelCase   | `createOrder`      |
| Variables       | camelCase   | `orderCount`       |
| Constants       | UPPER_SNAKE | `MAX_RETRY`        |
| Files           | kebab-case  | `order-service.ts` |
| Flutter Widgets | PascalCase  | `OrderCard`        |

### التعليقات

```typescript
/**
 * خدمة إدارة الطلبات
 *
 * @description تتعامل مع جميع عمليات الطلبات
 * @example
 * const order = await orderService.createOrder(dto);
 */
class OrderService {
  /**
   * إنشاء طلب جديد
   *
   * @param dto - بيانات الطلب
   * @returns الطلب المنشأ
   * @throws OrderException إذا فشل الإنشاء
   */
  async createOrder(dto: CreateOrderDto): Promise<Order> {
    // التحقق من توفر المطعم
    // ...
  }
}
```

### ESLint/Prettier

```bash
# تشغيل Linter
npm run lint

# إصلاح المشاكل تلقائياً
npm run lint:fix

# تنسيق الكود
npm run format
```

---

## 🔄 عملية Pull Request

### الخطوات

```bash
# 1. تأكد من أنك على أحدث نسخة
git fetch upstream
git rebase upstream/main

# 2. أنشئ فرعاً للميزة
git checkout -b feature/amazing-feature

# 3. اعمل تغييراتك
# ...

# 4. أضف الاختبارات
npm run test

# 5. تأكد من Lint
npm run lint

# 6. احفظ التغييرات
git add .
git commit -m "feat: add amazing feature"

# 7. ارفع التغييرات
git push origin feature/amazing-feature

# 8. افتح Pull Request على GitHub
```

### صيغة Commit Messages

نتبع [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

#### الأنواع

| النوع      | الوصف           |
| ---------- | --------------- |
| `feat`     | ميزة جديدة      |
| `fix`      | إصلاح خطأ       |
| `docs`     | تغييرات التوثيق |
| `style`    | تنسيق الكود     |
| `refactor` | إعادة هيكلة     |
| `test`     | إضافة اختبارات  |
| `chore`    | مهام صيانة      |

#### أمثلة

```bash
feat(orders): add order tracking functionality

fix(auth): resolve token refresh issue

docs(readme): update installation instructions

test(users): add unit tests for user service
```

### قائمة التحقق للـ PR

- [ ] الكود يتبع معايير المشروع
- [ ] الاختبارات مكتوبة ومرت بنجاح
- [ ] التوثيق محدث (إن لزم)
- [ ] Commit messages تتبع المعايير
- [ ] لا توجد conflicts مع main
- [ ] تمت مراجعة الكود ذاتياً

---

## 🐛 الإبلاغ عن الأخطاء

### قبل الإبلاغ

1. ابحث في [Issues](https://github.com/moien-delivery/moien-delivery/issues) الموجودة
2. تأكد من أنك تستخدم أحدث نسخة
3. تحقق من التوثيق

### كيفية الإبلاغ

استخدم هذا القالب:

```markdown
## وصف الخطأ

وصف واضح ومختصر للخطأ.

## خطوات إعادة الإنتاج

1. اذهب إلى '...'
2. اضغط على '....'
3. مرر لأسفل إلى '....'
4. شاهد الخطأ

## السلوك المتوقع

وصف واضح لما توقعت حدوثه.

## لقطات الشاشة

إذا أمكن، أضف لقطات شاشة.

## البيئة

- النظام: [مثال: Windows 11]
- المتصفح: [مثال: Chrome 120]
- النسخة: [مثال: 1.0.0]

## معلومات إضافية

أي سياق إضافي عن المشكلة.
```

---

## 💡 اقتراح الميزات

### كيفية الاقتراح

```markdown
## وصف الميزة

وصف واضح ومختصر للميزة المقترحة.

## المشكلة التي تحلها

لماذا هذه الميزة مفيدة؟

## الحل المقترح

كيف تتخيل عمل الميزة؟

## البدائل المدروسة

هل فكرت في حلول أخرى؟

## معلومات إضافية

رسومات، mockups، أو أي شيء آخر.
```

---

## 🏷️ التسميات (Labels)

| التسمية            | الوصف           |
| ------------------ | --------------- |
| `bug`              | خطأ في الكود    |
| `enhancement`      | ميزة جديدة      |
| `documentation`    | تحسين التوثيق   |
| `good first issue` | مناسب للمبتدئين |
| `help wanted`      | نحتاج مساعدة    |
| `priority: high`   | أولوية عالية    |
| `priority: low`    | أولوية منخفضة   |
| `wontfix`          | لن يتم إصلاحه   |

---

## 🎉 شكراً لك!

كل مساهمة، مهما كانت صغيرة، تساعد في تحسين Moien Delivery للجميع.

إذا كان لديك أي أسئلة، لا تتردد في:

- فتح Issue
- التواصل عبر [Discord](https://discord.gg/moiendelivery)
- إرسال بريد إلى [contributors@moiendelivery.com](mailto:contributors@moiendelivery.com)

---

<div align="center">

**صنع بـ ❤️ من قبل مجتمع Moien Delivery**

[🔙 العودة للرئيسية](README.md)

</div>
