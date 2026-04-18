# System Architecture Diagram - المرافق (Al-Murafeq)

## 1️⃣ High-Level Package Diagram (Three-Layer Architecture)

```mermaid
graph TB
    subgraph "Presentation Layer"
        WEB["🌐 Web Client<br/>React.js"]
        MOBILE["📱 Mobile Client<br/>React Native"]
    end

    subgraph "Business Logic Layer (Facade Pattern)"
        FACADE["🎭 API Facade<br/>Controllers & Routers"]
        
        subgraph "Services"
            USERSERVICE["👤 User Service"]
            PLACESERVICE["📍 Place Service"]
            REVIEWSERVICE["⭐ Review Service"]
            AUTHSERVICE["🔐 Auth Service"]
        end

        subgraph "Business Objects"
            USEROBJ["User Entity"]
            PLACEOBJ["Place Entity"]
            REVIEWOBJ["Review Entity"]
            AMENITYOBJ["Amenity Entity"]
        end

        FACADE -->|Route Requests| USERSERVICE
        FACADE -->|Route Requests| PLACESERVICE
        FACADE -->|Route Requests| REVIEWSERVICE
        FACADE -->|Route Requests| AUTHSERVICE

        USERSERVICE -->|Manage| USEROBJ
        PLACESERVICE -->|Manage| PLACEOBJ
        REVIEWSERVICE -->|Manage| REVIEWOBJ
        PLACEOBJ -->|Contains| AMENITYOBJ
    end

    subgraph "Data Access Layer"
        USERREPO["User Repository"]
        PLACEREPO["Place Repository"]
        REVIEWREPO["Review Repository"]
        AMENITYREPO["Amenity Repository"]

        subgraph "Database"
            DB["🗄️ PostgreSQL Database"]
        end

        USERREPO -->|Query/Update| DB
        PLACEREPO -->|Query/Update| DB
        REVIEWREPO -->|Query/Update| DB
        AMENITYREPO -->|Query/Update| DB
    end

    WEB -->|HTTP/REST| FACADE
    MOBILE -->|HTTP/REST| FACADE

    USERSERVICE -->|Use| USERREPO
    PLACESERVICE -->|Use| PLACEREPO
    PLACESERVICE -->|Use| AMENITYREPO
    REVIEWSERVICE -->|Use| REVIEWREPO

    style WEB fill:#4A90E2,color:#fff
    style MOBILE fill:#4A90E2,color:#fff
    style FACADE fill:#F5A623,color:#fff
    style USERSERVICE fill:#7ED321
    style PLACESERVICE fill:#7ED321
    style REVIEWSERVICE fill:#7ED321
    style AUTHSERVICE fill:#7ED321
    style USEROBJ fill:#50E3C2,color:#fff
    style PLACEOBJ fill:#50E3C2,color:#fff
    style REVIEWOBJ fill:#50E3C2,color:#fff
    style AMENITYOBJ fill:#50E3C2,color:#fff
    style USERREPO fill:#BD10E0,color:#fff
    style PLACEREPO fill:#BD10E0,color:#fff
    style REVIEWREPO fill:#BD10E0,color:#fff
    style AMENITYREPO fill:#BD10E0,color:#fff
    style DB fill:#FF6B6B,color:#fff
```

