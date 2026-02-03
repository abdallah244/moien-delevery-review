# 🔌 WebSockets (Socket.IO) | أحداث الوقت الحقيقي

<div align="center">

[![Socket.IO](https://img.shields.io/badge/Socket.IO-Realtime-black.svg)]()
[![Backend](https://img.shields.io/badge/Backend-NestJS-red.svg)]()

**توثيق أحداث الوقت الحقيقي (Realtime) في Moien Delivery**

</div>

---

## 🎯 نظرة عامة

يستخدم المشروع **Socket.IO** لتمرير تحديثات المستخدمين إلى لوحة الأدمن بشكل فوري (بدون Refresh).

- **Server**: NestJS Socket.IO Gateway
- **Namespace**: `/users`
- **هدف الاستخدام الحالي**: تحديث جدول المستخدمين في لوحة الأدمن عند إنشاء/حذف مستخدم

> ملاحظة: الـ WebSocket لا يمر عبر `api/v1` (هو مسار Socket.IO مستقل على نفس الـ host/port).

---

## 🔗 الاتصال (Connection)

### رابط الاتصال في التطوير

- `http://localhost:3000/users`

### مثال (TypeScript)

```ts
import { io } from "socket.io-client";

const socket = io("http://localhost:3000/users", {
  transports: ["websocket"],
});

socket.on("connect", () => {
  console.log("connected", socket.id);
});

socket.on("connect_error", () => {
  console.log("connection failed");
});
```

---

## 📣 الأحداث (Events)

### `users.created`

يتم بث الحدث عند نجاح تسجيل مستخدم جديد عبر:

- `POST /api/v1/users/register`

**Payload (PublicUserDto):**

```json
{
  "id": "uuid",
  "fullName": "Ahmed Mohamed Ali",
  "countryCode": "LU",
  "countryName": "Luxembourg",
  "phone": "+352691234567",
  "email": "user@example.com",
  "createdAt": "2026-02-01T12:00:00.000Z"
}
```

### `users.deleted`

يتم بث الحدث عند نجاح حذف مستخدم عبر:

- `DELETE /api/v1/users/:id`

**Payload:**

```json
{ "id": "uuid" }
```

---

## 🛡️ CORS

الـ Gateway يسمح بالاتصال من:

- `http://localhost:4200`
- `http://127.0.0.1:4200`
- `http://localhost:5173`
- `http://127.0.0.1:5173`

---

## 🧩 أين يُستخدم هذا في الويب؟

لوحة الأدمن في واجهة الويب تستمع للأحداث لتحديث الجدول بشكل لحظي:

- عند `users.created`: إضافة المستخدم أعلى الجدول (مع منع التكرار).
- عند `users.deleted`: حذف المستخدم من الجدول.

---

<div align="center">

[🔙 العودة للتوثيق](README.md)

</div>
