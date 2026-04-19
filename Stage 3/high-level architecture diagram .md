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
        DB2[(NoSql<br/>Firebase Firestore)]
        FS[(Firebase Storage)]
    end

    subgraph External["External APIs"]
        MOYASAR["Payment API<br/>(Moyasar)"]
    end

    %% Client ↔ Backend
    FE <-->|"HTTP Requests / JSON"| BE

    %% Backend ↔ MySQL
    BE <-->|"CRUD Operations<br/>(Structured Data)"| DB

    %% Backend ↔ Firebase Firestore
    BE <-->|"Admin SDK<br/>Validation / Background Jobs"| DB2

    %% Backend ↔ Firebase Storage
    BE <-->|"Upload / Retrieve Files<br/>(Images, Documents)"| FS

    %% Client ↔ Firebase Firestore
    FE <-->|"Realtime Sync<br/>Auth / Notifications"| DB2

    %% Client ↔ Firebase Storage (optional direct upload)
    FE <-->|"File Uploads (optional)"| FS

    %% Backend ↔ Payment Gateway
    BE <-->|"Payment Processing<br/>Create / Verify Transactions"| MOYASAR
```

## Architecture Overview

The system is structured as a full-stack web platform with a clear separation between components:

| Component | Technology | Description |
|----------|------------|-------------|
| Frontend | React | Single Page Application handling UI and real-time interactions |
| Backend | Flask | REST API server handling business logic, authentication, and system operations |
| Database (SQL) | MySQL | Relational database for structured and transactional data |
| Database (NoSQL) | Firestore | NoSQL database used for real-time sync, notifications, and lightweight data |
| File Storage | Firebase Storage | Cloud storage for images, documents, and user-uploaded files |
| Authentication | Firebase Auth | Token-based authentication and session management |
| Payment Gateway | Moyasar | Secure payment processing for Visa, MasterCard, and Mada |


### Data Flow

```mermaid
sequenceDiagram
    participant User
    participant React as React Frontend
    participant FirebaseAuth
    participant FirebaseDB
    participant FirebaseStorage
    participant API as Flask Backend
    participant MySql
    participant Moyasar as Moyasar API

    %% User Interaction
    User->>React: Interact with UI

    %% Authentication
    React->>FirebaseAuth: Login / Register
    FirebaseAuth-->>React: Auth Token

    %% Secure API Request
    React->>API: HTTP Request + Firebase Token
    API->>FirebaseAuth: Verify Token
    FirebaseAuth-->>API: Token Valid

    %% File Upload Flow
    React->>FirebaseStorage: Upload File (images/docs)
    FirebaseStorage-->>React: File URL

    %% Business Logic
    API->>MySql: Query / Update Structured Data
    MySql-->>API: DB Response

    %% Payment Flow
    alt Payment Required
        API->>Moyasar: Create / Verify Payment
        Moyasar-->>API: Payment Result
    end

    %% Response
    API-->>React: JSON Response
    React-->>User: Update UI

    %% Realtime Updates
    FirebaseDB-->>React: Realtime Sync / Notifications
```
