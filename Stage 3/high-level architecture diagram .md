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
    participant abc1 Frontend
    participant abc2 Boot API
    participant abc3
    participant abc4 API

    User->>abc1: Interact with UI
    abc1->>abc2: HTTP Request (JSON)
    abc2->>abc3: Query/Update Data
    abc3-->>abc2: Response
    
    alt Payment Required
        abc2->>abc4: Process Payment
        abc4-->>abc2: Payment Result
    end
    
    abc2-->>abc1: JSON Response
    abc1-->>User: Update UI
```
