# MVP System Architecture
## 1️⃣ High-Level Package Diagram (Three-Layer Architecture)

```mermaid
flowchart LR
    subgraph Client["Client Layer"]
        FE["Frontend<br/>(abc....)"]
    end

    subgraph Server["Server Layer"]
        BE["Backend<br/>(Python + FLASK)"]
    end

    subgraph Data["Data Layer"]
        DB[(abc....<br/>Database)]
    end

    subgraph External["External APIs"]
        MOYASAR["Payment API<br/>(Moyasar)"]
    end

    FE <-->|"HTTP Requests / JSON"| BE
    BE <-->|"Queries / Responses"| DB
    BE <-->|"Payment Processing"| MOYASAR
```

### Data Flow

```mermaid
sequenceDiagram
    participant User
    participant React as React Frontend
    participant API
    participant DB as PostgreSQL
    participant Moyasar as Moyasar API

    User->>React: Interact with UI
    React->>API: HTTP Request (JSON)
    API->>DB: Query/Update Data
    DB-->>API: Response
    
    alt Payment Required
        API->>Moyasar: Process Payment
        Moyasar-->>API: Payment Result
    end
    
    API-->>React: JSON Response
    React-->>User: Update UI
```
