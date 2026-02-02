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

| الصفحة             | الوصف                  | الحالة         |
| ------------------ | ---------------------- | -------------- |
| 🏠 Landing Page    | الصفحة الرئيسية للزوار | ⏳ قيد التطوير |
| 🔐 Admin Dashboard | لوحة تحكم المشرفين     | ⏳ قيد التطوير |

### المكونات المكتملة

| المكون            | الوصف              | الحالة   |
| ----------------- | ------------------ | -------- |
| 🧭 Navbar         | شريط التنقل        | ✅ مكتمل |
| 🦶 Footer         | التذييل            | ✅ مكتمل |
| 🌍 Language Modal | نافذة اختيار اللغة | ✅ مكتمل |
| 🎨 Theme Modal    | نافذة اختيار الثيم | ✅ مكتمل |

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
│   │   │   ├── navbar/
│   │   │   │   ├── navbar.ts
│   │   │   │   ├── navbar.html
│   │   │   │   └── navbar.css
│   │   │   ├── footer/
│   │   │   │   ├── footer.ts
│   │   │   │   ├── footer.html
│   │   │   │   └── footer.css
│   │   │   ├── language-modal/
│   │   │   │   ├── language-modal.ts
│   │   │   │   ├── language-modal.html
│   │   │   │   └── language-modal.css
│   │   │   └── theme-modal/
│   │   │       ├── theme-modal.ts
│   │   │       ├── theme-modal.html
│   │   │       └── theme-modal.css
│   │   │
│   │   ├── services/                 # ✅ الخدمات المكتملة
│   │   │   ├── language.ts          # إدارة الترجمات
│   │   │   └── theme.ts             # إدارة الثيمات
│   │   │
│   │   ├── core/                     # النواة (قيد التطوير)
│   │   │   ├── guards/
│   │   │   │   ├── auth.guard.ts
│   │   │   │   └── admin.guard.ts
│   │   │   ├── interceptors/
│   │   │   │   ├── auth.interceptor.ts
│   │   │   │   └── error.interceptor.ts
│   │   │   ├── services/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── api.service.ts
│   │   │   │   └── storage.service.ts
│   │   │   └── models/
│   │   │       ├── user.model.ts
│   │   │       └── api-response.model.ts
│   │   │
│   │   ├── shared/                   # المشترك
│   │   │   ├── components/
│   │   │   │   ├── header/
│   │   │   │   ├── footer/
│   │   │   │   ├── sidebar/
│   │   │   │   └── loading/
│   │   │   ├── directives/
│   │   │   ├── pipes/
│   │   │   └── utils/
│   │   │
│   │   ├── features/                 # الميزات
│   │   │   ├── landing/              # الصفحة الرئيسية
│   │   │   │   ├── hero/
│   │   │   │   ├── features/
│   │   │   │   ├── how-it-works/
│   │   │   │   ├── restaurants/
│   │   │   │   ├── download/
│   │   │   │   └── contact/
│   │   │   │
│   │   │   └── admin/                # لوحة التحكم
│   │   │       ├── dashboard/
│   │   │       ├── users/
│   │   │       ├── restaurants/
│   │   │       ├── orders/
│   │   │       ├── drivers/
│   │   │       ├── promotions/
│   │   │       ├── reports/
│   │   │       └── settings/
│   │   │
│   │   ├── layout/
│   │   │   ├── landing-layout/
│   │   │   └── admin-layout/
│   │   │
│   │   └── app.routes.ts
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
// src/app/features/landing/landing.component.ts

@Component({
  selector: "app-landing",
  standalone: true,
  imports: [HeroComponent, FeaturesComponent, HowItWorksComponent, RestaurantsShowcaseComponent, DownloadAppComponent, ContactComponent],
  template: `
    <app-hero />
    <app-features />
    <app-how-it-works />
    <app-restaurants-showcase />
    <app-download-app />
    <app-contact />
  `,
})
export class LandingComponent {}
```

### لوحة التحكم الإدارية

```typescript
// src/app/features/admin/dashboard/dashboard.component.ts

@Component({
  selector: "app-admin-dashboard",
  standalone: true,
  template: `
    <div class="grid grid-cols-4 gap-6">
      <!-- إحصائيات -->
      <app-stat-card title="الطلبات اليوم" [value]="stats().ordersToday" icon="shopping_cart" color="blue" />
      <app-stat-card title="الإيرادات" [value]="stats().revenue | currency: 'EUR'" icon="payments" color="green" />
      <app-stat-card title="المستخدمين الجدد" [value]="stats().newUsers" icon="person_add" color="purple" />
      <app-stat-card title="السائقين النشطين" [value]="stats().activeDrivers" icon="delivery_dining" color="orange" />
    </div>

    <!-- الرسوم البيانية -->
    <div class="grid grid-cols-2 gap-6 mt-6">
      <app-orders-chart [data]="ordersChartData()" />
      <app-revenue-chart [data]="revenueChartData()" />
    </div>

    <!-- الطلبات الأخيرة -->
    <app-recent-orders [orders]="recentOrders()" />
  `,
})
export class DashboardComponent {
  private dashboardService = inject(DashboardService);

  stats = this.dashboardService.stats;
  ordersChartData = this.dashboardService.ordersChartData;
  revenueChartData = this.dashboardService.revenueChartData;
  recentOrders = this.dashboardService.recentOrders;
}
```

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

---

## 🛣️ التوجيه

```typescript
// src/app/app.routes.ts

export const routes: Routes = [
  {
    path: "",
    component: LandingLayoutComponent,
    children: [
      { path: "", component: LandingComponent },
      { path: "about", component: AboutComponent },
      { path: "contact", component: ContactComponent },
    ],
  },
  {
    path: "admin",
    component: AdminLayoutComponent,
    canActivate: [authGuard, adminGuard],
    children: [
      { path: "", redirectTo: "dashboard", pathMatch: "full" },
      {
        path: "dashboard",
        loadComponent: () => import("./features/admin/dashboard/dashboard.component").then((m) => m.DashboardComponent),
      },
      {
        path: "users",
        loadComponent: () => import("./features/admin/users/users.component").then((m) => m.UsersComponent),
      },
      {
        path: "restaurants",
        loadComponent: () => import("./features/admin/restaurants/restaurants.component").then((m) => m.RestaurantsComponent),
      },
      {
        path: "orders",
        loadComponent: () => import("./features/admin/orders/orders.component").then((m) => m.OrdersComponent),
      },
    ],
  },
  { path: "**", redirectTo: "" },
];
```

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [📱 تطبيقات الموبايل](FRONTEND-MOBILE.md)

</div>
