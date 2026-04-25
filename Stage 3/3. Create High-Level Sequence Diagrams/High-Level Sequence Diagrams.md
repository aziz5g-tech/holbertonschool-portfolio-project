# High-Level Sequence Diagrams

## Use Case: Create Service Request (Service Seeker Requests Assistance)

```mermaid
sequenceDiagram
    participant User as Service Seeker
    participant Frontend as React Frontend
    participant API as Node.js Backend
    participant FirebaseAuth
    participant MySQL
    participant Firebase as Firebase (Realtime / Notifications)

    User->>Frontend: Fill service request form
    Frontend->>FirebaseAuth: Verify user token
    FirebaseAuth-->>Frontend: Token valid

    Frontend->>API: POST /requests (data + token)
    API->>FirebaseAuth: Verify token
    FirebaseAuth-->>API: User authenticated

    API->>MySQL: Save service request
    MySQL-->>API: Request created

    API->>Firebase: Send notification (new request created)
    Firebase-->>Frontend: Real-time update

    API-->>Frontend: Request confirmation
    Frontend-->>User: Show success message

```


## Use Case: Accept Request & Share Companion Details

```mermaid

    sequenceDiagram
    participant Companion
    participant Frontend as React Frontend
    participant API as Node.js Backend
    participant MySQL
    participant Firebase as Firebase (Realtime / Notifications)

    Companion->>Frontend: Accept service request
    Frontend->>API: POST /requests/{id}/accept

    API->>MySQL: Update request status = accepted
    MySQL-->>API: Status updated

    API->>MySQL: Fetch companion profile & contact details
    MySQL-->>API: Companion data

    API->>Firebase: Notify service seeker & elderly user
    Firebase-->>Frontend: Real-time notification

    API-->>Frontend: Return confirmation & details
    Frontend-->>Companion: Display request information
```

## Use Case: Real-Time Trip Tracking (Family Monitoring)
 
```mermaid

    sequenceDiagram
    participant Companion
    participant Frontend as React App
    participant API as Node.js Backend
    participant FirebaseDB as Firebase Firestore
    participant Family as Service Seeker

    Companion->>Frontend: Update trip status / location
    Frontend->>FirebaseDB: Send live location update

    FirebaseDB-->>Family: Real-time location update

    API->>FirebaseDB: Sync trip status (on_the_way / in_progress / completed)
    FirebaseDB-->>Family: Status notification

    Family-->>Family: View live tracking on map
```

