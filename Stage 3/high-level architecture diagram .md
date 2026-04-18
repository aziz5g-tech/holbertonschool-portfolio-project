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
