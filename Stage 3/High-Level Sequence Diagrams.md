Use Case: Create Service Request (Elderly User Requests Assistance)

sequenceDiagram
    participant User
    participant Frontend as React Frontend
    participant API as Flask Backend
    participant FirebaseAuth
    participant MySQL
    participant Firebase as Firebase (Realtime/Notifications)

    User->>Frontend: Fill service request form
    Frontend->>FirebaseAuth: Verify user token
    FirebaseAuth-->>Frontend: Token valid

    Frontend->>API: POST /create-request (data + token)
    API->>FirebaseAuth: Verify token
    FirebaseAuth-->>API: User authenticated

    API->>MySQL: Save service request
    MySQL-->>API: Request created

    API->>Firebase: Send notification (new request created)
    Firebase-->>Frontend: Real-time update

    API-->>Frontend: Request confirmation
    Frontend-->>User: Show success message






Use Case: Real-Time Trip Tracking (Family Monitoring)

    sequenceDiagram
    participant Companion
    participant Frontend as React Frontend
    participant API as Flask Backend
    participant MySQL
    participant Firebase

    Companion->>Frontend: Accept service request
    Frontend->>API: POST /accept-request

    API->>MySQL: Update request status = accepted
    MySQL-->>API: Updated

    API->>MySQL: Fetch companion profile + contact info
    MySQL-->>API: Companion details

    API->>Firebase: Notify user (request accepted)
    Firebase-->>Frontend: Real-time notification

    API-->>Frontend: Return companion contact details
    Frontend-->>Companion: Display request details



Use Case: Real-Time Trip Tracking (Family Monitoring)

    sequenceDiagram
    participant Companion
    participant Frontend as React App
    participant API as Flask Backend
    participant FirebaseDB as Firebase Realtime DB
    participant Family

    Companion->>Frontend: Update location / trip status
    Frontend->>FirebaseDB: Send live location update

    FirebaseDB-->>Family: Real-time location update

    API->>FirebaseDB: Sync trip status (in_progress/completed)
    FirebaseDB-->>Family: Notify status change

    Family-->>Family: View live tracking on map
