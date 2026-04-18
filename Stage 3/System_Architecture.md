# System Architecture Diagram - المرافق (Al-Murafeq)

## High-Level System Architecture

```mermaid
graph TB
    subgraph "Client Layer - طبقة العملاء"
        WEB["🌐 Web Application<br/>React.js"]
        MOBILE["📱 Mobile Application<br/>React Native"]
    end

    subgraph "API Gateway & Load Balancer"
        GATEWAY["API Gateway<br/>Express.js/REST API"]
    end

    subgraph "Backend Services - الخدمات الخلفية"
        AUTH["🔐 Authentication Service<br/>JWT/OAuth"]
        USER["👤 User Management Service<br/>Profiles & Preferences"]
        REQUEST["📋 Request Service<br/>Job Matching"]
        MATCH["🎯 Matching Engine<br/>Smart Assignment"]
        LOCATION["📍 Location Service<br/>GPS & Tracking"]
        RATING["⭐ Rating & Review Service"]
        NOTIFICATION["🔔 Notification Service<br/>Real-time Updates"]
    end

    subgraph "Data Layer - طبقة البيانات"
        DB["🗄️ Primary Database<br/>PostgreSQL"]
        CACHE["⚡ Cache Layer<br/>Redis"]
        STORAGE["📦 File Storage<br/>AWS S3/Cloud Storage"]
    end

    subgraph "External Services - الخدمات الخارجية"
        MAPS["🗺️ Maps Service<br/>Google Maps API"]
        SMS["📞 SMS Service<br/>Twilio/Local Provider"]
        EMAIL["📧 Email Service<br/>SendGrid/Local Provider"]
    end

    subgraph "Real-time Communication"
        WEBSOCKET["⚙️ WebSocket Server<br/>Socket.io"]
    end

    WEB -->|REST API Calls| GATEWAY
    MOBILE -->|REST API Calls| GATEWAY

    GATEWAY -->|Routes Requests| AUTH
    GATEWAY -->|Routes Requests| USER
    GATEWAY -->|Routes Requests| REQUEST
    GATEWAY -->|Routes Requests| MATCH
    GATEWAY -->|Routes Requests| LOCATION
    GATEWAY -->|Routes Requests| RATING
    GATEWAY -->|Routes Requests| NOTIFICATION

    AUTH -->|Validate Token| DB
    USER -->|Read/Write| DB
    USER -->|Profile Pictures| STORAGE
    REQUEST -->|Read/Write| DB
    MATCH -->|Read/Write| DB
    MATCH -->|Caching| CACHE
    LOCATION -->|Real-time Updates| WEBSOCKET
    LOCATION -->|Queries| MAPS
    LOCATION -->|Write| DB
    RATING -->|Read/Write| DB
    NOTIFICATION -->|Sends| SMS
    NOTIFICATION -->|Sends| EMAIL
    NOTIFICATION -->|Real-time| WEBSOCKET

    WEBSOCKET -->|Live Updates| WEB
    WEBSOCKET -->|Live Updates| MOBILE

    style WEB fill:#4A90E2
    style MOBILE fill:#4A90E2
    style GATEWAY fill:#F5A623
    style AUTH fill:#7ED321
    style USER fill:#7ED321
    style REQUEST fill:#7ED321
    style MATCH fill:#7ED321
    style LOCATION fill:#7ED321
    style RATING fill:#7ED321
    style NOTIFICATION fill:#7ED321
    style DB fill:#BD10E0
    style CACHE fill:#BD10E0
    style STORAGE fill:#BD10E0
    style MAPS fill:#50E3C2
    style SMS fill:#50E3C2
    style EMAIL fill:#50E3C2
    style WEBSOCKET fill:#FF6B6B
```

## Data Flow Description - تدفق البيانات

### 1. **User Registration & Authentication** (تسجيل المستخدم)
- المستخدم ينشئ حساب (مسن أو مرافق أو ابن/ابنة)
- البيانات تُخزن في قاعدة البيانات
- صورة شخصية تُرفع إلى Cloud Storage

### 2. **Service Request Flow** (تقديم طلب الخدمة)
- مستخدم يضع طلب مساعدة
- تحديد نوع المساعدة والموقع
- Request Service يسجل الطلب في قاعدة البيانات

### 3. **Matching Engine** (مطابقة المرافق المناسب)
- Matching Engine تحلل الطلب
- تبحث عن مرافقين متاحين
- تراعي التقييمات والتفضيلات المحفوظة
- تختار أنسب مرافق

### 4. **Real-time Location Tracking** (تتبع الموقع لحظياً)
- Location Service يتلقى إحداثيات GPS من المرافق
- تُرسل التحديثات عبر WebSocket
- المستخدم يرى الموقع على الخريطة فوراً

### 5. **Notifications** (إشعارات فورية)
- Notification Service ترسل SMS/Email
- تتضمن بيانات التواصل مع المرافق
- تحديثات الحالة لحظياً عبر WebSocket

### 6. **Rating & Review** (تقييم الخدمة)
- بعد انتهاء الخدمة
- المستخدم يقيّم الطرف الآخر
- التقييمات تُحسّن نظام المطابقة

## Key Features Mapping - خريطة الميزات

| Feature | المسار المعماري |
|---------|-----------------|
| **تحديد نوع المساعدة** | REQUEST → DB |
| **استلام بيانات المرافق** | NOTIFICATION → SMS/EMAIL |
| **تحديثات لحظية للحالة** | LOCATION → WEBSOCKET → MOBILE/WEB |
| **صورة المرافق** | STORAGE → USER SERVICE → MOBILE/WEB |
| **حفظ المفضلين** | USER → DB → MATCH |
| **تحديد الموقع على الخريطة** | LOCATION → MAPS API |
| **سجل الطلبات** | REQUEST → DB |
| **التقييمات** | RATING → DB |

## Technology Stack - المكدس التكنولوجي

**Frontend:**
- React.js / React Native
- Leaflet/Mapbox for Maps

**Backend:**
- Node.js + Express.js
- Socket.io for Real-time Communication

**Database:**
- PostgreSQL (Primary)
- Redis (Caching & Real-time data)

**External APIs:**
- Google Maps API
- Twilio/Local SMS Provider
- SendGrid/Local Email Provider

**Cloud Services:**
- AWS S3 or Similar for Image Storage
- Load Balancing & Hosting

