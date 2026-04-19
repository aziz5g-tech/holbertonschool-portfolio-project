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
    participant API
    participant MySql
    participant Moyasar as Moyasar API

    User->>React: Interact with UI
    React->>API: HTTP Request (JSON)
    API->>MySql: Query/Update Data
    MySql-->>API: Response
    
    alt Payment Required
        API->>Moyasar: Process Payment
        Moyasar-->>API: Payment Result
    end
    
    API-->>React: JSON Response
    React-->>User: Update UI
```
