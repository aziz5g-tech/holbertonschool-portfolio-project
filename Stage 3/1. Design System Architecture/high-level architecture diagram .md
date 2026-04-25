# MVP System Architecture
## High-Level Package Diagram (Three-Layer Architecture)

```mermaid
flowchart LR
    subgraph Client["Client Layer"]
        FE["Frontend<br/>(React)"]
    end

    subgraph Server["Server Layer"]
        BE["Backend<br/>(Node.js + Express)"]
    end

    subgraph Data["Data Layer"]
        DB[(MySQL<br/>Database)]
        DB2[(NoSQL<br/>Firebase Firestore)]
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

The system is designed as a full-stack web platform following a three-layer architecture that ensures scalability, maintainability, and clear separation of concerns.

The architecture consists of a client layer for user interaction, a server layer for business logic and system operations, and a data layer for data persistence and real-time services.

### System Components

| Component | Technology | Description |
|----------|------------|-------------|
| Frontend | React | Single Page Application (SPA) responsible for rendering the user interface and handling user interactions |
| Backend | Node.js (Express.js) | RESTful API server responsible for business logic, request validation, authentication, and integrations |
| Database (SQL) | MySQL | Relational database used for structured and transactional data |
| Database (NoSQL) | Firebase Firestore | NoSQL database used for real-time synchronization, notifications, and lightweight data |
| File Storage | Firebase Storage | Cloud-based storage for images, documents, and user-uploaded files |
| Authentication | Firebase Authentication | Secure token-based authentication and session management |
| Payment Gateway | Moyasar | Secure payment processing for Visa, MasterCard, and Mada |

### Architectural Principles

- **Separation of Concerns:** Each layer is responsible for a specific set of tasks, reducing coupling between components.
- **Scalability:** The backend and data layers can scale independently based on system load.
- **Security:** Authentication is handled using Firebase Authentication with token verification on the backend.
- **Extensibility:** The architecture allows for easy integration of additional services and features in future phases.
