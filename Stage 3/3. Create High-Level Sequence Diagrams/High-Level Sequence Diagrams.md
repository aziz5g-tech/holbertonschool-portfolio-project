# High-Level Sequence Diagrams

## Use Case 1: Create Service Request and Pay Before Submission
```mermaid
sequenceDiagram
    participant User as Service Seeker
    participant Frontend as React Frontend
    participant Backend as Node.js Backend
    participant MySQL
    participant Moyasar as Payment Gateway
    participant Firebase as Firebase Realtime

    User->>Frontend: Fill request form
    Frontend->>Backend: POST /api/v1/requests (JSON + JWT)
    Backend->>MySQL: Validate user, request data, beneficiary, health data, location
    MySQL-->>Backend: Validation result

    Backend->>Moyasar: Create payment charge
    Moyasar-->>Backend: Payment session / transaction info
    Backend-->>Frontend: Payment checkout data

    User->>Frontend: Complete payment
    Frontend->>Moyasar: Submit payment
    Moyasar-->>Backend: Webhook payment confirmation
    Backend->>MySQL: Save payment as paid
    Backend->>MySQL: Save request as pending companion response
    Backend->>Firebase: Send realtime notification
    Firebase-->>Frontend: Request status update
    Backend-->>Frontend: Request created successfully
```

## Use Case 2: Companion Accepts or Rejects the Request
```mermaid
sequenceDiagram
    participant Companion as Companion
    participant Frontend as React Frontend
    participant Backend as Node.js Backend
    participant MySQL
    participant Firebase as Firebase Realtime
    participant Moyasar as Payment Gateway

    Companion->>Frontend: Open pending request
    Frontend->>Backend: POST /api/v1/requests/{id}/accept-or-reject (JSON + JWT)
    Backend->>MySQL: Verify companion approval, request status, and availability
    MySQL-->>Backend: Verification result

    alt Companion accepts
        Backend->>MySQL: Update request status = accepted
        Backend->>MySQL: Create trip session
        Backend->>Firebase: Create chat / notify user / start tracking session
        Firebase-->>Frontend: Realtime acceptance update
        Backend-->>Frontend: Acceptance confirmed
    else Companion rejects
        Backend->>MySQL: Update request status = rejected
        Backend->>Moyasar: Trigger refund
        Moyasar-->>Backend: Refund confirmation
        Backend->>MySQL: Save refund record
        Backend->>Firebase: Notify user of rejection and refund status
        Firebase-->>Frontend: Realtime rejection update
        Backend-->>Frontend: Rejection confirmed
    end
```

## Use Case 3: Real-Time Trip Tracking and Trip Completion
```mermaid
sequenceDiagram
    participant Companion as Companion
    participant Frontend as React Frontend
    participant Backend as Node.js Backend
    participant Firebase as Firebase Realtime
    participant MySQL
    participant User as Service Seeker

    Companion->>Frontend: Share live location
    Frontend->>Firebase: Write trip_live_tracking update
    Firebase-->>User: Live location update on map

    Backend->>MySQL: Update trip session status
    MySQL-->>Backend: Status saved
    Backend->>Firebase: Send notification and presence update
    Firebase-->>Frontend: Realtime trip status update

    Companion->>Frontend: End trip
    Frontend->>Backend: POST /api/v1/trips/{request_id}/end (JSON + JWT)
    Backend->>MySQL: Close trip session and request status
    Backend->>Firebase: Notify user trip completed
    Firebase-->>User: Trip completed notification
    Backend-->>Frontend: Trip end confirmation
```

---

## sequence diagrams showing interactions between components

### 1) User Login
```mermaid
sequenceDiagram
    participant FE as Front-end
    participant BE as Back-end
    participant DB as Database

    FE->>BE: POST /auth/login (phone/email + password)
    BE->>DB: Validate user credentials
    DB-->>BE: User record + status
    BE-->>FE: JWT token + user profile
```

### 2) Create Service Request
```mermaid
sequenceDiagram
    participant FE as Front-end
    participant BE as Back-end
    participant DB as Database

    FE->>BE: POST /requests (form data)
    BE->>DB: Insert new request (pending)
    DB-->>BE: request_id + saved row
    BE-->>FE: Request created successfully
```

### 3) Companion Accept Request
```mermaid
sequenceDiagram
    participant FE as Front-end
    participant BE as Back-end
    participant DB as Database

    FE->>BE: POST /requests/{id}/accept
    BE->>DB: Update request status = accepted
    DB-->>BE: Update confirmation
    BE-->>FE: Acceptance confirmed + updated status
```
