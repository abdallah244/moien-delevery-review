# 📱 توثيق تطبيقات الموبايل | Mobile Apps Documentation

<div align="center">

[![Flutter](https://img.shields.io/badge/Flutter-3.10-02569B.svg)]()
[![Dart](https://img.shields.io/badge/Dart-3.10-0175C2.svg)]()
[![Clean Architecture](https://img.shields.io/badge/Architecture-Clean-green.svg)]()

**توثيق تطبيقات Flutter لـ Moien Delivery**

</div>

---

## 📋 جدول المحتويات

- [نظرة عامة](#-نظرة-عامة)
- [هيكل المشروع](#-هيكل-المشروع)
- [التطبيقات](#-التطبيقات)
- [إدارة الحالة](#-إدارة-الحالة)
- [الميزات المشتركة](#-الميزات-المشتركة)
- [الاختبار](#-الاختبار)
- [البناء والنشر](#-البناء-والنشر)

---

## 🎯 نظرة عامة

### التطبيقات

| التطبيق           | الوصف                  | المستخدمون    |
| ----------------- | ---------------------- | ------------- |
| 👤 Customer App   | تطبيق طلب الطعام       | العملاء       |
| 🍴 Restaurant App | إدارة الطلبات والقوائم | أصحاب المطاعم |
| 🚗 Driver App     | إدارة التوصيلات        | السائقين      |

### التقنيات

| التقنية             | الاستخدام      |
| ------------------- | -------------- |
| Flutter 3.10        | إطار العمل     |
| Dart 3.10           | لغة البرمجة    |
| BLoC/Cubit          | إدارة الحالة   |
| GetIt               | حقن التبعيات   |
| Dio                 | HTTP Client    |
| go_router           | التنقل         |
| Hive                | التخزين المحلي |
| firebase_messaging  | الإشعارات      |
| google_maps_flutter | الخرائط        |

---

## 📁 هيكل المشروع

```
mfrontend/
├── lib/
│   ├── main.dart                      # نقطة الدخول
│   ├── app.dart                       # تكوين التطبيق
│   │
│   ├── core/                          # النواة المشتركة
│   │   ├── api/
│   │   │   ├── api_client.dart        # عميل HTTP
│   │   │   ├── api_endpoints.dart     # نقاط النهاية
│   │   │   ├── api_interceptors.dart  # المعترضات
│   │   │   └── api_exceptions.dart    # الاستثناءات
│   │   │
│   │   ├── config/
│   │   │   ├── app_config.dart        # إعدادات التطبيق
│   │   │   ├── theme_config.dart      # السمات
│   │   │   └── route_config.dart      # التنقل
│   │   │
│   │   ├── di/
│   │   │   └── injection.dart         # حقن التبعيات
│   │   │
│   │   ├── constants/
│   │   │   ├── app_colors.dart
│   │   │   ├── app_strings.dart
│   │   │   └── app_sizes.dart
│   │   │
│   │   ├── utils/
│   │   │   ├── validators.dart
│   │   │   ├── formatters.dart
│   │   │   └── extensions.dart
│   │   │
│   │   └── error/
│   │       ├── failures.dart
│   │       └── exceptions.dart
│   │
│   ├── shared/                        # المكونات المشتركة
│   │   ├── widgets/
│   │   │   ├── buttons/
│   │   │   │   ├── primary_button.dart
│   │   │   │   └── social_button.dart
│   │   │   ├── inputs/
│   │   │   │   ├── text_field.dart
│   │   │   │   └── phone_field.dart
│   │   │   ├── cards/
│   │   │   │   ├── restaurant_card.dart
│   │   │   │   └── order_card.dart
│   │   │   └── loading/
│   │   │       └── shimmer.dart
│   │   │
│   │   ├── models/
│   │   │   ├── user_model.dart
│   │   │   ├── restaurant_model.dart
│   │   │   ├── order_model.dart
│   │   │   └── address_model.dart
│   │   │
│   │   └── services/
│   │       ├── auth_service.dart
│   │       ├── location_service.dart
│   │       ├── notification_service.dart
│   │       └── storage_service.dart
│   │
│   ├── features/                      # الميزات
│   │   │
│   │   ├── auth/                      # المصادقة (مشترك)
│   │   │   ├── presentation/
│   │   │   │   ├── screens/
│   │   │   │   │   ├── login_screen.dart
│   │   │   │   │   ├── register_screen.dart
│   │   │   │   │   └── forgot_password_screen.dart
│   │   │   │   ├── widgets/
│   │   │   │   └── bloc/
│   │   │   │       ├── auth_bloc.dart
│   │   │   │       ├── auth_event.dart
│   │   │   │       └── auth_state.dart
│   │   │   ├── domain/
│   │   │   │   ├── entities/
│   │   │   │   ├── repositories/
│   │   │   │   └── usecases/
│   │   │   └── data/
│   │   │       ├── models/
│   │   │       ├── repositories/
│   │   │       └── datasources/
│   │   │
│   │   ├── customer/                  # تطبيق العملاء
│   │   │   ├── home/
│   │   │   ├── restaurants/
│   │   │   ├── restaurant_details/
│   │   │   ├── cart/
│   │   │   ├── checkout/
│   │   │   ├── orders/
│   │   │   ├── order_tracking/
│   │   │   ├── favorites/
│   │   │   ├── profile/
│   │   │   └── addresses/
│   │   │
│   │   ├── restaurant/                # تطبيق المطاعم
│   │   │   ├── dashboard/
│   │   │   ├── orders/
│   │   │   ├── menu/
│   │   │   ├── analytics/
│   │   │   ├── settings/
│   │   │   └── profile/
│   │   │
│   │   └── driver/                    # تطبيق السائقين
│   │       ├── home/
│   │       ├── deliveries/
│   │       ├── delivery_details/
│   │       ├── navigation/
│   │       ├── earnings/
│   │       ├── history/
│   │       └── profile/
│   │
│   └── l10n/                          # الترجمة
│       ├── app_ar.arb
│       ├── app_lb.arb
│       └── app_en.arb
│
├── assets/
│   ├── images/
│   ├── icons/
│   ├── animations/
│   └── fonts/
│
├── test/
│   ├── unit/
│   ├── widget/
│   └── integration/
│
├── android/
├── ios/
├── web/
├── macos/
├── windows/
├── linux/
│
├── pubspec.yaml
└── analysis_options.yaml
```

---

## 📱 التطبيقات

### تطبيق العملاء (Customer App)

#### الشاشات الرئيسية

```dart
// lib/features/customer/home/presentation/screens/home_screen.dart

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomScrollView(
        slivers: [
          // شريط البحث
          SliverAppBar(
            title: SearchBar(),
            floating: true,
          ),
          // الموقع الحالي
          SliverToBoxAdapter(
            child: LocationSelector(),
          ),
          // الفئات
          SliverToBoxAdapter(
            child: CategoriesSection(),
          ),
          // العروض
          SliverToBoxAdapter(
            child: PromotionsCarousel(),
          ),
          // المطاعم القريبة
          SliverToBoxAdapter(
            child: NearbyRestaurantsSection(),
          ),
          // المطاعم المميزة
          SliverToBoxAdapter(
            child: FeaturedRestaurantsSection(),
          ),
        ],
      ),
      bottomNavigationBar: CustomerBottomNav(),
    );
  }
}
```

#### تتبع الطلب

```dart
// lib/features/customer/order_tracking/presentation/screens/tracking_screen.dart

class OrderTrackingScreen extends StatelessWidget {
  final String orderId;

  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => getIt<OrderTrackingBloc>()
        ..add(StartTracking(orderId)),
      child: Scaffold(
        body: Stack(
          children: [
            // الخريطة
            OrderTrackingMap(),
            // معلومات الطلب
            DraggableScrollableSheet(
              builder: (context, controller) {
                return OrderTrackingDetails(
                  scrollController: controller,
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

---

### تطبيق المطاعم (Restaurant App)

#### لوحة التحكم

```dart
// lib/features/restaurant/dashboard/presentation/screens/dashboard_screen.dart

class RestaurantDashboardScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('لوحة التحكم'),
        actions: [
          // حالة المطعم (متاح/غير متاح)
          RestaurantStatusSwitch(),
        ],
      ),
      body: Column(
        children: [
          // إحصائيات اليوم
          TodayStatsCard(),
          // الطلبات الجديدة
          Expanded(
            child: NewOrdersList(),
          ),
        ],
      ),
      bottomNavigationBar: RestaurantBottomNav(),
    );
  }
}
```

#### إدارة الطلبات

```dart
// lib/features/restaurant/orders/presentation/bloc/orders_bloc.dart

class RestaurantOrdersBloc extends Bloc<OrdersEvent, OrdersState> {
  final GetPendingOrdersUseCase getPendingOrders;
  final AcceptOrderUseCase acceptOrder;
  final RejectOrderUseCase rejectOrder;
  final UpdateOrderStatusUseCase updateStatus;

  RestaurantOrdersBloc({
    required this.getPendingOrders,
    required this.acceptOrder,
    required this.rejectOrder,
    required this.updateStatus,
  }) : super(OrdersInitial()) {
    on<LoadOrders>(_onLoadOrders);
    on<AcceptOrder>(_onAcceptOrder);
    on<RejectOrder>(_onRejectOrder);
    on<UpdateOrderStatus>(_onUpdateStatus);
  }

  Future<void> _onAcceptOrder(
    AcceptOrder event,
    Emitter<OrdersState> emit,
  ) async {
    final result = await acceptOrder(event.orderId);
    result.fold(
      (failure) => emit(OrderError(failure.message)),
      (order) => emit(OrderAccepted(order)),
    );
  }
}
```

---

### تطبيق السائقين (Driver App)

#### الشاشة الرئيسية

```dart
// lib/features/driver/home/presentation/screens/driver_home_screen.dart

class DriverHomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<DriverBloc, DriverState>(
      builder: (context, state) {
        return Scaffold(
          body: Stack(
            children: [
              // الخريطة
              DriverMap(),
              // حالة التوفر
              Positioned(
                top: 50,
                right: 20,
                child: AvailabilityToggle(),
              ),
              // الطلبات المتاحة
              if (state is DriverOnline)
                DraggableScrollableSheet(
                  builder: (context, controller) {
                    return AvailableDeliveriesList(
                      scrollController: controller,
                    );
                  },
                ),
              // التوصيل الحالي
              if (state is DriverOnDelivery)
                CurrentDeliveryCard(delivery: state.delivery),
            ],
          ),
        );
      },
    );
  }
}
```

---

## 🔄 إدارة الحالة

### BLoC Pattern

```dart
// lib/features/auth/presentation/bloc/auth_bloc.dart

class AuthBloc extends Bloc<AuthEvent, AuthState> {
  final LoginUseCase loginUseCase;
  final RegisterUseCase registerUseCase;
  final LogoutUseCase logoutUseCase;
  final GetCurrentUserUseCase getCurrentUser;

  AuthBloc({
    required this.loginUseCase,
    required this.registerUseCase,
    required this.logoutUseCase,
    required this.getCurrentUser,
  }) : super(AuthInitial()) {
    on<CheckAuthStatus>(_onCheckAuthStatus);
    on<LoginRequested>(_onLoginRequested);
    on<RegisterRequested>(_onRegisterRequested);
    on<LogoutRequested>(_onLogoutRequested);
  }

  Future<void> _onLoginRequested(
    LoginRequested event,
    Emitter<AuthState> emit,
  ) async {
    emit(AuthLoading());

    final result = await loginUseCase(
      LoginParams(
        email: event.email,
        password: event.password,
      ),
    );

    result.fold(
      (failure) => emit(AuthError(failure.message)),
      (user) => emit(Authenticated(user)),
    );
  }
}
```

### Events

```dart
// lib/features/auth/presentation/bloc/auth_event.dart

abstract class AuthEvent extends Equatable {
  const AuthEvent();

  @override
  List<Object?> get props => [];
}

class CheckAuthStatus extends AuthEvent {}

class LoginRequested extends AuthEvent {
  final String email;
  final String password;

  const LoginRequested({
    required this.email,
    required this.password,
  });

  @override
  List<Object?> get props => [email, password];
}

class RegisterRequested extends AuthEvent {
  final String email;
  final String password;
  final String name;
  final String phone;

  const RegisterRequested({
    required this.email,
    required this.password,
    required this.name,
    required this.phone,
  });
}

class LogoutRequested extends AuthEvent {}
```

### States

```dart
// lib/features/auth/presentation/bloc/auth_state.dart

abstract class AuthState extends Equatable {
  const AuthState();

  @override
  List<Object?> get props => [];
}

class AuthInitial extends AuthState {}

class AuthLoading extends AuthState {}

class Authenticated extends AuthState {
  final User user;

  const Authenticated(this.user);

  @override
  List<Object?> get props => [user];
}

class Unauthenticated extends AuthState {}

class AuthError extends AuthState {
  final String message;

  const AuthError(this.message);

  @override
  List<Object?> get props => [message];
}
```

---

## 🔧 الميزات المشتركة

### حقن التبعيات

```dart
// lib/core/di/injection.dart

final getIt = GetIt.instance;

Future<void> initDependencies() async {
  // External
  getIt.registerLazySingleton(() => Dio());
  getIt.registerLazySingleton(() => InternetConnectionChecker());

  // Core
  getIt.registerLazySingleton<ApiClient>(
    () => ApiClient(getIt()),
  );

  // Data Sources
  getIt.registerLazySingleton<AuthRemoteDataSource>(
    () => AuthRemoteDataSourceImpl(getIt()),
  );
  getIt.registerLazySingleton<AuthLocalDataSource>(
    () => AuthLocalDataSourceImpl(),
  );

  // Repositories
  getIt.registerLazySingleton<AuthRepository>(
    () => AuthRepositoryImpl(
      remoteDataSource: getIt(),
      localDataSource: getIt(),
      networkInfo: getIt(),
    ),
  );

  // Use Cases
  getIt.registerLazySingleton(() => LoginUseCase(getIt()));
  getIt.registerLazySingleton(() => RegisterUseCase(getIt()));
  getIt.registerLazySingleton(() => LogoutUseCase(getIt()));

  // BLoCs
  getIt.registerFactory(
    () => AuthBloc(
      loginUseCase: getIt(),
      registerUseCase: getIt(),
      logoutUseCase: getIt(),
      getCurrentUser: getIt(),
    ),
  );
}
```

### التنقل

```dart
// lib/core/config/route_config.dart

final goRouter = GoRouter(
  initialLocation: '/splash',
  redirect: (context, state) {
    final isAuthenticated = getIt<AuthService>().isAuthenticated;
    final isAuthRoute = state.matchedLocation.startsWith('/auth');

    if (!isAuthenticated && !isAuthRoute) {
      return '/auth/login';
    }
    if (isAuthenticated && isAuthRoute) {
      return '/home';
    }
    return null;
  },
  routes: [
    GoRoute(
      path: '/splash',
      builder: (context, state) => SplashScreen(),
    ),
    GoRoute(
      path: '/auth',
      routes: [
        GoRoute(
          path: 'login',
          builder: (context, state) => LoginScreen(),
        ),
        GoRoute(
          path: 'register',
          builder: (context, state) => RegisterScreen(),
        ),
      ],
    ),
    ShellRoute(
      builder: (context, state, child) => MainShell(child: child),
      routes: [
        GoRoute(
          path: '/home',
          builder: (context, state) => HomeScreen(),
        ),
        GoRoute(
          path: '/orders',
          builder: (context, state) => OrdersScreen(),
        ),
        GoRoute(
          path: '/profile',
          builder: (context, state) => ProfileScreen(),
        ),
      ],
    ),
  ],
);
```

---

## 🧪 الاختبار

### Unit Tests

```dart
// test/unit/auth/login_usecase_test.dart

void main() {
  late LoginUseCase useCase;
  late MockAuthRepository mockRepository;

  setUp(() {
    mockRepository = MockAuthRepository();
    useCase = LoginUseCase(mockRepository);
  });

  test('should return User when login is successful', () async {
    // Arrange
    final user = User(id: '1', email: 'test@test.com', name: 'Test');
    when(mockRepository.login(any, any))
        .thenAnswer((_) async => Right(user));

    // Act
    final result = await useCase(
      LoginParams(email: 'test@test.com', password: 'password'),
    );

    // Assert
    expect(result, Right(user));
    verify(mockRepository.login('test@test.com', 'password'));
  });
}
```

### Widget Tests

```dart
// test/widget/login_screen_test.dart

void main() {
  testWidgets('should show error when login fails', (tester) async {
    // Arrange
    final mockBloc = MockAuthBloc();
    when(mockBloc.state).thenReturn(AuthError('Invalid credentials'));

    // Act
    await tester.pumpWidget(
      BlocProvider<AuthBloc>.value(
        value: mockBloc,
        child: MaterialApp(home: LoginScreen()),
      ),
    );

    // Assert
    expect(find.text('Invalid credentials'), findsOneWidget);
  });
}
```

---

## 📦 البناء والنشر

### Android

```bash
# إصدار Debug
flutter build apk --debug

# إصدار Release
flutter build apk --release

# Bundle للـ Play Store
flutter build appbundle --release
```

### iOS

```bash
# إصدار Debug
flutter build ios --debug

# إصدار Release
flutter build ios --release

# للـ App Store
flutter build ipa --release
```

### تكوين Firebase

```bash
# تثبيت FlutterFire CLI
dart pub global activate flutterfire_cli

# تكوين المشروع
flutterfire configure
```

---

<div align="center">

[🔙 العودة للتوثيق](README.md) | [🌐 واجهة الويب](FRONTEND-WEB.md)

</div>
