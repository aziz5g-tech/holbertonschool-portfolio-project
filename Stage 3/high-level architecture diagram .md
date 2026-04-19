# MVP System Architecture
## 1️⃣ High-Level Package Diagram (Three-Layer Architecture)

```mermaid
flowchart LR
    subgraph Client["Client Layer"]
        FE["Frontend<br/>(React)"]
    end

    subgraph Server["Server Layer"]
        BE["Backend<br/>(FLASK)"]
    end

    subgraph Data["Data Layer"]
        DB[(MySql<br/>Database)]
        DB2[(NoSql<br/>Firebase)]
    end

    subgraph External["External APIs"]
        MOYASAR["Payment API<br/>(Moyasar)"]
    end

    %% Client ↔ Backend
    FE <-->|"HTTP Requests / JSON"| BE

    %% Backend ↔ MySQL
    BE <-->|"CRUD Operations<br/>(Structured Data)"| DB

    %% Backend ↔ Payment Gateway
    BE <-->|"Payment Processing<br/>Create / Verify Transactions"| MOYASAR

    %% Client ↔ Firebase
    FE <-->|"Realtime Sync<br/>Auth / Notifications"| DB2

    %% Backend ↔ Firebase
    BE <-->|"Admin SDK<br/>Validation / Background Jobs"| DB2
```

### Data Flow

```mermaid
sequenceDiagram
    participant User
    participant React as React Frontend
    participant Firebase
    participant API as Flask Backend
    participant MySql
    participant Moyasar as Moyasar API

    %% User Interaction
    User->>React: Interact with UI

    %% Authentication & Realtime
    React->>Firebase: Authenticate User
    Firebase-->>React: Auth Token / User State

    %% Secure API Request
    React->>API: HTTP Request + Firebase Token
    API->>Firebase: Verify Token (Admin SDK)
    Firebase-->>API: Token Valid

    %% Business Data Flow
    API->>MySql: Query / Update Structured Data
    MySql-->>API: DB Response

    %% Optional Payment Flow
    alt Payment Required
        API->>Moyasar: Create / Verify Payment
        Moyasar-->>API: Payment Result
    end

    %% Final Response
    API-->>React: JSON Response
    React-->>User: Update UI

    %% Realtime Updates
    Firebase-->>React: Realtime Sync / Notifications
```
