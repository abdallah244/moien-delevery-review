# 🌐 توثيق واجهة الويب | Web Frontend Documentation

<div align="center">

[![Angular](https://img.shields.io/badge/Angular-21-dd0031.svg)]()
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178c6.svg)]()
[![Font Awesome](https://img.shields.io/badge/Font%20Awesome-6.5.1-528DD7.svg)]()

**توثيق واجهة الويب لـ Moien Delivery**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [هيكل المشروع](#-هيكل-المشروع)
- [المكونات المكتملة](#-المكونات-المكتملة)
- [نظام الترجمة](#-نظام-الترجمة)
- [نظام الثيمات](#-نظام-الثيمات)
- [إدارة الحالة](#-إدارة-الحالة)
- [التوجيه](#-التوجيه)

---

## 🎯 نظرة عامة

### الصفحات

| الصفحة             | الوصف                                | الحالة              |
| ------------------ | ------------------------------------ | ------------------- |
| 🏠 Landing Page    | الصفحة الرئيسية للزوار               | ✅ تم تنفيذها (MVP) |
| ℹ️ About Page      | صفحة التعريف (About)                 | ✅ مكتمل            |
| 🍴 Restaurants     | قائمة المطاعم + تفاصيل المطعم/المنيو | ✅ تم تنفيذها (MVP) |
| 🛒 Cart            | السلة                                | ✅ تم تنفيذها (MVP) |
| 💳 Checkout        | الدفع (MVP)                          | ✅ تم تنفيذها (MVP) |
| 📍 Order Tracking  | تتبع الطلب (MVP)                     | ✅ تم تنفيذها (MVP) |
| 🔐 Admin Login     | صفحة تسجيل دخول المسؤول              | ✅ مكتمل            |
| 📊 Admin Dashboard | لوحة تحكم المسؤول                    | ✅ مكتمل            |
| 👥 Admin Users     | إدارة المستخدمين للمسؤول             | ✅ مكتمل            |
| ⚙️ Admin Settings  | إعدادات النظام                       | ✅ مكتمل            |
| 🖼️ About Images    | إدارة صور صفحة About                 | ✅ مكتمل            |
| 👤 User Profile    | صفحة الملف الشخصي للمستخدم           | ✅ تم تنفيذها (MVP) |
| 📜 Legal Pages     | صفحات قانونية (سياسات)               | ✅ مكتمل            |

### المكونات المكتملة

| المكون             | الوصف                  | الحالة   |
| ------------------ | ---------------------- | -------- |
| 🧭 Navbar          | شريط التنقل للمستخدمين | ✅ مكتمل |
| 🦶 Footer          | التذييل للمستخدمين     | ✅ مكتمل |
| 🌍 Language Modal  | نافذة اختيار اللغة     | ✅ مكتمل |
| 🎨 Theme Modal     | نافذة اختيار الثيم     | ✅ مكتمل |
| 🔒 Admin Navbar    | شريط تنقل المسؤول      | ✅ مكتمل |
| 🔒 Admin Footer    | تذييل المسؤول          | ✅ مكتمل |
| 🍞 Toast Container | حاوية الإشعارات        | ✅ مكتمل |

### الخدمات (Services)

| الخدمة                 | الوصف               | الحالة   |
| ---------------------- | ------------------- | -------- |
| `auth.ts`              | المصادقة العامة     | ✅ مكتمل |
| `user-auth.ts`         | مصادقة المستخدم     | ✅ مكتمل |
| `user-session.ts`      | إدارة جلسة المستخدم | ✅ مكتمل |
| `admin-staff.ts`       | إدارة موظفي الأدمن  | ✅ مكتمل |
| `admin-language.ts`    | لغة لوحة التحكم     | ✅ مكتمل |
| `admin-master-code.ts` | كود التحكم الرئيسي  | ✅ مكتمل |
| `language.ts`          | الترجمة (7 لغات)    | ✅ مكتمل |
| `theme.ts`             | الثيمات (4 ثيمات)   | ✅ مكتمل |
| `toast.ts`             | إشعارات Toast       | ✅ مكتمل |
| `supportTickets.ts`    | تذاكر الدعم الفني   | ✅ مكتمل |

### التقنيات

| التقنية            | الاستخدام    |
| ------------------ | ------------ |
| Angular 21         | إطار العمل   |
| TypeScript 5.9     | لغة البرمجة  |
| Angular Signals    | إدارة الحالة |
| CSS Variables      | نظام الثيمات |
| Font Awesome 6.5.1 | الأيقونات    |

---

## ✅ المكونات المكتملة

### 🧭 شريط التنقل (Navbar)

تصميم احترافي مشابه لـ **Wolt** و **Uber Eats**:

```
┌─────────────────────────────────────────────────────────────────┐
│ [Logo]  📍 Location ▼  [═══════ Search... ═══════]  [Login] [SignUp] │
└─────────────────────────────────────────────────────────────────┘
```

**المميزات:**

- ✅ لوجو المنصة على اليسار
- ✅ محدد الموقع مع أيقونة
- ✅ خانة بحث متوسعة مع Animation سلسة
- ✅ أزرار تسجيل الدخول والتسجيل
- ✅ بعد تسجيل الدخول: تتحول الأزرار إلى Avatar chip مع قائمة منسدلة
- ✅ عناصر القائمة: Profile، Redeem code، Language submenu، Contact support (placeholder)، Logout
- ✅ دعم كامل للـ Themes عبر CSS Variables
- ✅ تصميم Responsive

**الملفات:**

```
components/navbar/
├── navbar.ts       # Component logic
├── navbar.html     # Template
└── navbar.css      # Styles with CSS Variables
```

### 🦶 التذييل (Footer)

تذييل شامل بتصميم متعدد الأعمدة:

```
┌─────────────────────────────────────────────────────────────────┐
│ [App Store] [Google Play]                                       │
├─────────────────────────────────────────────────────────────────┤
│ Partner    │ Company      │ Products    │ Support   │ Follow Us │
│ ---------- │ ------------ │ ----------- │ --------- │ --------- │
│ Restaurant │ About        │ Moien...    │ Help      │ [f] [i]   │
│ Deliver    │ Jobs         │ Moien...    │ FAQ       │ [t] [t]   │
│ Merchant   │ Contact      │             │           │           │
├─────────────────────────────────────────────────────────────────┤
│ [📍 Location] [🌍 English] [🌙 Theme]                            │
├─────────────────────────────────────────────────────────────────┤
│ © 2026 Moien Delivery | Privacy | Terms | Accessibility | © |   │
└─────────────────────────────────────────────────────────────────┘
```

**الملفات:**

```
components/footer/
├── footer.ts       # Component logic with services
├── footer.html     # Template with Font Awesome icons
└── footer.css      # Styles with CSS Variables
```

### 🌍 نافذة اللغات (Language Modal)

نافذة اختيار لغة احترافية:

**اللغات المدعومة:**

| اللغة          | الكود | العلم |
| -------------- | ----- | ----- |
| English        | `en`  | 🇬🇧    |
| Français       | `fr`  | 🇫🇷    |
| Lëtzebuergesch | `lb`  | 🇱🇺    |
| Deutsch        | `de`  | 🇩🇪    |
| Italiano       | `it`  | 🇮🇹    |
| Português      | `pt`  | 🇵🇹    |
| Español        | `es`  | 🇪🇸    |

**الملفات:**

```
components/language-modal/
├── language-modal.ts
├── language-modal.html
└── language-modal.css
```

### 🎨 نافذة الثيمات (Theme Modal)

نافذة اختيار ثيم بتصميم شبكي 2x2:

**الثيمات المتاحة:**

| الثيم         | الأيقونة                | الوصف               |
| ------------- | ----------------------- | ------------------- |
| Auto          | `fa-circle-half-stroke` | يتبع إعدادات النظام |
| Light         | `fa-sun`                | الوضع الفاتح        |
| Dark          | `fa-moon`               | الوضع الداكن        |
| High Contrast | `fa-eye`                | تباين عالي للوصولية |

**الملفات:**

```
components/theme-modal/
├── theme-modal.ts
├── theme-modal.html
└── theme-modal.css
```

---

## 🌍 نظام الترجمة (i18n System)

### Language Service

```typescript
// services/language.ts

interface Language {
  code: string;    // 'en', 'fr', 'lb', etc.
  name: string;    // Display name
  flag: string;    // Emoji flag
}

// استخدام الخدمة
@Component({...})
export class MyComponent {
  private lang = inject(LanguageService);

  // الحصول على الترجمة
  text = this.lang.t('navbar.signIn');

  // تغيير اللغة
  changeLanguage() {
    this.lang.setLanguage('fr');
  }
}
```

### الترجمات المتاحة

```typescript
// مثال على الترجمات
translations = {
  en: {
    navbar: {
      location: "Enter delivery address",
      search: "Search restaurants or cuisines...",
      signIn: "Sign in",
      signUp: "Sign up",
    },
    footer: {
      partner: "Partner with us",
      // ...
    },
  },
  // نفس الهيكل للغات الأخرى
};
```

---

## 🎨 نظام الثيمات (Theme System)

### Theme Service

```typescript
// services/theme.ts

type Theme = 'auto' | 'light' | 'dark' | 'high-contrast';

> ملاحظة تنفيذية: حالياً الوضع الافتراضي للواجهة هو **dark** عند أول تحميل (First paint). ويمكن للمستخدم تغييره من Theme Modal.

// استخدام الخدمة
@Component({...})
export class MyComponent {
  private theme = inject(ThemeService);

  // الحصول على الثيم الحالي
  currentTheme = this.theme.currentTheme();

  // تغيير الثيم
  setDark() {
    this.theme.setTheme('dark');
  }
}
```

### CSS Variables

```css
/* styles.css */

/* الثيم الداكن */
.theme-dark {
  --bg-primary: #1b1b1b;
  --bg-secondary: #242424;
  --bg-tertiary: #2d2d2d;
  --bg-hover: #363636;
  --text-primary: #ffffff;
  --text-secondary: #a0a0a0;
  --border-color: #3d3d3d;
  --primary-color: #00c2e8;
  --primary-hover: #00a8cc;
}

/* الثيم الفاتح */
.theme-light {
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --bg-tertiary: #e9ecef;
  --bg-hover: #dee2e6;
  --text-primary: #1a1a1a;
  --text-secondary: #6c757d;
  --border-color: #dee2e6;
  --primary-color: #009de0;
  --primary-hover: #0077b6;
}

/* ثيم التباين العالي */
.theme-high-contrast {
  --bg-primary: #000000;
  --bg-secondary: #1a1a1a;
  --bg-tertiary: #2a2a2a;
  --bg-hover: #3a3a3a;
  --text-primary: #ffffff;
  --text-secondary: #e0e0e0;
  --border-color: #ffffff;
  --primary-color: #00d4ff;
  --primary-hover: #00b8e6;
}
```

### استخدام CSS Variables في المكونات

```css
/* navbar.css */
.navbar {
  background: var(--bg-primary);
  border-bottom: 1px solid var(--border-color);
  transition:
    background 0.3s ease,
    border-color 0.3s ease;
}

.navbar-logo {
  color: var(--text-primary);
}

.search-input {
  background: var(--bg-secondary);
  color: var(--text-primary);
  border: 1px solid var(--border-color);
}

.btn-primary {
  background: var(--primary-color);
}

.btn-primary:hover {
  background: var(--primary-hover);
}
```

---

## 📁 هيكل المشروع

```
wfrontend/
├── src/
│   ├── app/
│   │   ├── components/               # ✅ المكونات المكتملة
│   │   │   ├── navbar/              # شريط تنقل المستخدمين
│   │   │   │   ├── navbar.ts
│   │   │   │   ├── navbar.html
│   │   │   │   └── navbar.css
│   │   │   ├── footer/              # تذييل المستخدمين
│   │   │   │   ├── footer.ts
│   │   │   │   ├── footer.html
│   │   │   │   └── footer.css
│   │   │   ├── admin-navbar/        # ✅ شريط تنقل المسؤول
│   │   │   │   ├── admin-navbar.ts
│   │   │   │   ├── admin-navbar.html
│   │   │   │   └── admin-navbar.css
│   │   │   ├── admin-footer/        # ✅ تذييل المسؤول
│   │   │   │   ├── admin-footer.ts
│   │   │   │   ├── admin-footer.html
│   │   │   │   └── admin-footer.css
│   │   │   ├── language-modal/
│   │   │   │   ├── language-modal.ts
│   │   │   │   ├── language-modal.html
│   │   │   │   └── language-modal.css
│   │   │   ├── theme-modal/
│   │   │   │   ├── theme-modal.ts
│   │   │   │   ├── theme-modal.html
│   │   │   │   └── theme-modal.css
│   │   │   └── toast-container/     # ✅ حاوية الإشعارات
│   │   │       ├── toast-container.ts
│   │   │       ├── toast-container.html
│   │   │       └── toast-container.css
│   │   │
│   │   ├── services/                 # ✅ الخدمات المكتملة
│   │   │   ├── language.ts          # إدارة الترجمات
│   │   │   ├── theme.ts             # إدارة الثيمات
│   │   │   ├── auth.ts              # ✅ المصادقة
│   │   │   └── toast.ts             # ✅ الإشعارات
│   │   │
│   │   ├── guards/                   # ✅ حراس المسارات
│   │   │   └── auth.guard.ts        # Auth Guard + Login Guard
│   │   │
│   │   ├── layouts/                  # ✅ تخطيطات الصفحات
│   │   │   ├── user-layout/         # تخطيط صفحات المستخدمين
│   │   │   │   ├── user-layout.ts
│   │   │   │   ├── user-layout.html
│   │   │   │   └── user-layout.css
│   │   │   └── admin-layout/        # تخطيط صفحات المسؤول
│   │   │       ├── admin-layout.ts
│   │   │       ├── admin-layout.html
│   │   │       └── admin-layout.css
│   │   │
│   │   ├── pages/                    # ✅ الصفحات
│   │   │   └── admin/
│   │   │       ├── login/           # صفحة تسجيل دخول المسؤول
│   │   │       │   ├── login.ts
│   │   │       │   ├── login.html
│   │   │       │   └── login.css
│   │   │       └── dashboard/       # لوحة تحكم المسؤول
│   │   │           ├── admin-dashboard.ts
│   │   │           ├── admin-dashboard.html
│   │   │           └── admin-dashboard.css
│   │   │
│   │   │       └── system-settings/  # إعدادات السيستم (Admin Staff)
│   │   │
│   │   └── user/
│   │       └── profile/              # ✅ My Profile page
│   │           ├── user-profile.ts
│   │           ├── user-profile.html
│   │           └── user-profile.css
│   │   │
│   │   └── app.routes.ts            # المسارات
│   │
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── i18n/
│   │       ├── ar.json
│   │       └── lb.json
│   │
│   └── styles/
│       ├── _variables.scss
│       └── _mixins.scss
│
├── angular.json
├── package.json
└── tsconfig.json
```

---

## 🧩 المكونات الرئيسية

### الصفحة الرئيسية (Landing Page)

```typescript
// src/app/pages/landing/landing.ts

@Component({
  selector: "app-landing",
  imports: [CommonModule],
  templateUrl: "./landing.v2.html",
  styleUrl: "./landing.v2.css",
})
export class Landing {}
```

### لوحة التحكم الإدارية (Admin Dashboard)

اللوحة الحالية تركيزها الأساسي على **إدارة المستخدمين**:

- عرض جدول المستخدمين (الاسم/الإيميل/البلد/الهاتف/الحالة/تاريخ الإنشاء).
- تحديثات لحظية عبر Socket.IO على namespace `/users`:
  - `users.created`, `users.updated`, `users.deleted`
- إجراءات:
  - **Ban** مع نافذة (Modal) لكتابة سبب الحظر (بالإنجليزية).
  - **Unban**.
  - **Delete**.

الملفات:

```
src/app/pages/admin/dashboard/
├── admin-dashboard.ts
├── admin-dashboard.html
└── admin-dashboard.css
```

> ملاحظة: يوجد جزء "Under Construction" لباقي أجزاء الداشبورد حالياً.

### صفحة My Profile (User)

صفحة الملف الشخصي صممت بواجهة مشابهة لـ Wolt مع الحفاظ على نفس ألوان الثيم:

- Hero header + زر Contact support (حالياً placeholder).
- Tabs: Personal info / Payment methods / Addresses / Order history / Earn credits / Redeem code / Settings.
- داخل تبويب Personal info:
  - Summary row (Avatar + الاسم/الإيميل/الهاتف) + تغيير الصورة.
  - تعديل الاسم.
  - Email verification + Phone verification (WhatsApp).

ملاحظات تخص التابات الجديدة:

- **Redeem code**: شاشة أنيقة وسريعة لإدخال الكود مع Validation بسيط ورسائل Inline. (التطبيق الحالي يعرض Coming Soon لأن endpoint الخاص بالـ redeem لم يتم ربطه بعد).
- **Earn credits**: Coming Soon لكن بتصميم أفضل (Cards توضّح طرق كسب الـ credits + زر تواصل مع الدعم).
- **Settings → Delete account**: الحذف يتم عبر Modal يطلب كلمة المرور للتأكيد قبل حذف الحساب.

الملفات:

```
src/app/pages/user/profile/
├── user-profile.ts
├── user-profile.html
└── user-profile.css
```

> تنبيه: يوجد حد (budget) لحجم CSS لكل Component في بناء الإنتاج. تم ضبطه حالياً على `anyComponentStyle: warning=15kB / error=30kB` لتفادي التحذيرات المتكررة مع الحفاظ على حد منطقي.

---

## 📊 إدارة الحالة

### Signal-Based Store

```typescript
// src/app/core/store/admin.store.ts

@Injectable({ providedIn: "root" })
export class AdminStore {
  // State
  private _users = signal<User[]>([]);
  private _restaurants = signal<Restaurant[]>([]);
  private _orders = signal<Order[]>([]);
  private _loading = signal(false);

  // Selectors
  users = this._users.asReadonly();
  restaurants = this._restaurants.asReadonly();
  orders = this._orders.asReadonly();
  loading = this._loading.asReadonly();

  // Computed
  pendingRestaurants = computed(() => this._restaurants().filter((r) => r.status === "pending"));

  todayOrders = computed(() => this._orders().filter((o) => isToday(new Date(o.createdAt))));

  // Actions
  async loadUsers() {
    this._loading.set(true);
    try {
      const users = await this.api.getUsers();
      this._users.set(users);
    } finally {
      this._loading.set(false);
    }
  }
}
```

### User Session (localStorage)

واجهة الويب تحفظ اللغة والجلسة في `localStorage`:

- `moien-lang`: كود اللغة الحالية.
- `moien-user-session`: بيانات المستخدم بعد تسجيل الدخول.

صفحة **My Profile** تعرض بيانات الجلسة المحلية حالياً (بدون endpoint "me").

---

## 🛣️ التوجيه

```typescript
// src/app/app.routes.ts

export const routes: Routes = [
  // Admin Routes - No User Navbar/Footer
  {
    path: "admin",
    children: [
      {
        path: "login",
        loadComponent: () => import("./pages/admin/login/admin-login").then((m) => m.AdminLoginComponent),
        canActivate: [loginGuard],
      },
      {
        path: "dashboard",
        loadComponent: () => import("./pages/admin/dashboard/admin-dashboard").then((m) => m.AdminDashboardComponent),
        canActivate: [authGuard],
      },
      {
        path: "promotions",
        loadComponent: () => import("./pages/admin/promotions/admin-promotions").then((m) => m.AdminPromotionsComponent),
        canActivate: [authGuard],
      },
      { path: "", redirectTo: "login", pathMatch: "full" },
    ],
  },

  // User Routes
  {
    path: "",
    loadComponent: () => import("./layouts/user-layout/user-layout").then((m) => m.UserLayoutComponent),
    children: [
      {
        path: "",
        redirectTo: "restaurants",
        pathMatch: "full",
      },
      {
        path: "home",
        loadComponent: () => import("./pages/landing/landing").then((m) => m.Landing),
      },
      {
        path: "restaurants",
        loadComponent: () => import("./pages/restaurants/restaurants").then((m) => m.RestaurantsPageComponent),
      },
      {
        path: "profile",
        loadComponent: () => import("./pages/user/profile/user-profile").then((m) => m.UserProfileComponent),
        canActivate: [userAuthGuard],
      },
    ],
  },

  { path: "**", redirectTo: "" },
];
```

---

## 🔐 نظام المصادقة

### Auth Service

```typescript
// src/app/services/auth.ts

@Injectable({ providedIn: "root" })
export class AuthService {
  // حالة تسجيل الدخول
  isAuthenticated(): boolean;

  // الحصول على المستخدم الحالي
  getCurrentUser(): User | null;

  // تسجيل الدخول
  login(email: string, password: string): Observable<LoginResult>;

  // تسجيل الخروج
  logout(): void;
}
```

### بيانات الدخول الافتراضية

```
Email: admin@moien.com
Password: 123456
```

### Rate Limiting

- **الحد الأقصى:** 5 محاولات
- **مدة الحظر:** 15 دقيقة

---

## 🔒 حراس المسارات (Guards)

### Auth Guard

يحمي صفحات الـ Admin من الوصول غير المصرح:

```typescript
// src/app/guards/auth.guard.ts

export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  const router = inject(Router);

  if (auth.isAuthenticated()) {
    return true;
  }

  return router.createUrlTree(["/admin/login"]);
};
```

### Login Guard

يمنع الوصول لصفحة تسجيل الدخول بعد المصادقة:

```typescript
export const loginGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  const router = inject(Router);

  if (!auth.isAuthenticated()) {
    return true;
  }

  return router.createUrlTree(["/admin/dashboard"]);
};
```

---

## 🍞 نظام الإشعارات (Toast)

### Toast Service

```typescript
// src/app/services/toast.ts

@Injectable({ providedIn: "root" })
export class ToastService {
  // إشعار نجاح
  success(message: string, duration?: number): void;

  // إشعار خطأ
  error(message: string, duration?: number): void;

  // إشعار معلومات
  info(message: string, duration?: number): void;

  // إشعار تحذير
  warning(message: string, duration?: number): void;
}
```

### الاستخدام

```typescript
@Component({...})
export class MyComponent {
  private toast = inject(ToastService);

  onSuccess() {
    this.toast.success('تم الحفظ بنجاح!');
  }

  onError() {
    this.toast.error('حدث خطأ، يرجى المحاولة لاحقاً');
  }
}
```

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [📱 تطبيقات الموبايل](FRONTEND-MOBILE.md)

</div>
