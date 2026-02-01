# 🌐 توثيق واجهة الويب | Web Frontend Documentation

<div align="center">

[![Angular](https://img.shields.io/badge/Angular-21-dd0031.svg)]()
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178c6.svg)]()

**توثيق واجهة الويب لـ Moien Delivery**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [هيكل المشروع](#-هيكل-المشروع)
- [المكونات الرئيسية](#-المكونات-الرئيسية)
- [إدارة الحالة](#-إدارة-الحالة)
- [التوجيه](#-التوجيه)

---

## 🎯 نظرة عامة

### الصفحات

| الصفحة             | الوصف                  |
| ------------------ | ---------------------- |
| 🏠 Landing Page    | الصفحة الرئيسية للزوار |
| 🔐 Admin Dashboard | لوحة تحكم المشرفين     |

### التقنيات

| التقنية          | الاستخدام    |
| ---------------- | ------------ |
| Angular 21       | إطار العمل   |
| TypeScript 5.9   | لغة البرمجة  |
| Angular Signals  | إدارة الحالة |
| TailwindCSS      | التنسيق      |
| Angular Material | مكتبة UI     |

---

## 📁 هيكل المشروع

```
wfrontend/
├── src/
│   ├── app/
│   │   ├── core/                     # النواة
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
