graph TD
    %% تعريف الطبقات (Layers) - لتنظيم الرسمة
    subgraph Client_Layer [طالب الخدمة / القريب (React/Mobile)]
        Seeker[تطبيق طالب الخدمة]
        Relative[تطبيق القريب]
    end

    subgraph Companion_Layer [المرافق (React/Mobile)]
        CompanionApp[تطبيق المرافق]
    end

    subgraph Internet [الإنترنت]
        LB[موازن الحمل - Load Balancer]
    end

    subgraph Application_Layer [طبقة التطبيق - السيرفر]
        API[Backend API Server<br/>(Spring Boot / Node.js)]
        Auth[نظام التحقق<br/>(JWT / Firebase Auth)]
    end

    subgraph Data_Layer [طبقة البيانات]
        DB[(PostgreSQL Database)]
        Redis[(Redis Cache)]
        Storage[Cloud Storage<br/>(صور المرافقين)]
    end

    subgraph External_Services [خدمات خارجية]
        Notify[Firebase Cloud Messaging<br/>(تحديثات الرحلة اللحظية)]
        Maps[Google Maps API<br/>(تتبع الرحلة وتحديد المواقع)]
    end

    %% تعريف الاتصالات وتدفق البيانات (arrows with annotations)
    
    %% من المستخدم للباكيند
    Seeker -->|1. تسجيل دخول<br/>2. طلب مرافق (تحديد نوع المساعدة/الموقع)| LB
    Relative -->|تتبع حالة الرحلة (Real-time)| LB
    
    %% من المرافق للباكيند
    CompanionApp -->|1. تسجيل دخول<br/>2. قبول الطلب<br/>3. تحديث حالة الرحلة (بداية/نهاية)| LB
    
    %% من موازن الحمل للباكيند
    LB --> API
    API <-->|تحقق من الصلاحيات (Role-based)| Auth
    
    %% اتصالات الباكيند الداخلية
    API <-->|استعلام/تحديث بيانات (طلب، مستخدم، تقييم)| DB
    API <-->|بيانات التتبع الجغرافي السريع| Redis
    API <-->|رفع/جلب صور المرافقين (US-004)| Storage
    
    %% اتصالات الباكيند الخارجية
    API -->|إرسال إشعارات (قبول طلب، تحديث حالة) (US-002, US-003)| Notify
    API -->|حساب المسافة، الاتجاهات (Set destination) (SH-02)| Maps
    
    %% اتصالات الواجهة الأمامية بالخارج (اختياري، للوضوح)
    CompanionApp -.->|جلب الخرائط| Maps
    Notify -.->|تنبيه لحظي| Relative
