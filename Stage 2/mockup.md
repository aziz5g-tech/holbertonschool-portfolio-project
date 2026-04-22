# Mockup - Rafiq Platform

هذا الملف مخصص لتجميع صور الموك اب ومخطط التدفق داخل GitHub.

## أماكن صور الموك اب
ضع الصور داخل نفس المجلد (`Stage 2/`) أو مجلد `Stage 2/assets/` ثم حدّث الروابط أدناه.

### 1) الصفحة الرئيسية
![Home Mockup](./assets/mockup-home.png)

### 2) تسجيل الدخول
![Login Mockup](./assets/mockup-login.png)

### 3) إنشاء حساب
![Register Mockup](./assets/mockup-register.png)

### 4) طلب الخدمة
![Request Service Mockup](./assets/mockup-request-service.png)

### 5) كن رفيقًا
![Provider Onboarding Mockup](./assets/mockup-provider-onboarding.png)

### 6) متابعة الرحلة
![Trip Tracking Mockup](./assets/mockup-trip-tracking.png)

## UX Flow (Mermaid)
```mermaid
flowchart TD
    A[Landing Page] --> B[Login / Register]
    B --> C{نوع الحساب}
    C -->|طالب خدمة| D[Request Service]
    C -->|مقدم خدمة| E[Provider Onboarding]
    D --> F[Calculate Price]
    F --> G[Payment]
    G --> H[Trip Tracking]
    H --> I[Map + ETA + Chat]
```

## ملاحظات
- GitHub يعرض Mermaid مباشرة داخل Markdown.
- إذا ما ظهرت الصور، تأكد من مسار الملفات داخل الريبو.
- يفضل تسمية الصور بدون مسافات لتجنب مشاكل الروابط.
