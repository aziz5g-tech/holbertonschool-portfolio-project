# Portfolio Project - Technical Documentation (Stage 3)

This document is the final Stage 3 summary. It consolidates the outputs from all Stage 3 tasks and provides links to detailed documents where content is large.

---

## 1) User Stories and Mockups

### User Stories (Prioritized)
The project uses four personas with prioritized stories following Must Have / Should Have / Could Have:
- Elderly User
- Service Seeker
- Companion
- Admin

MVP priorities focus on:
- account/profile creation and verification,
- service request creation and assignment,
- real-time status updates,
- companion operations,
- admin approval and issue handling.

Out-of-scope MVP items are clearly defined (e.g., transportation service ownership, advanced AI/video features).

Read more: [0. Define User Stories and Mockups/User Stories.md](0.%20Define%20User%20Stories%20and%20Mockups/User%20Stories.md)

### Mockups
Mockups are referenced in the Stage 3 structure.

Read more: [0. Define User Stories and Mockups/Mockups.md](0.%20Define%20User%20Stories%20and%20Mockups/Mockups.md)

---

## 2) System Architecture (High-Level)

The system follows a three-layer architecture:
- **Client Layer:** React frontend.
- **Server Layer:** Node.js + Express backend.
- **Data Layer:** MySQL + Firebase Firestore + Firebase Storage.
- **External Services:** Moyasar (payments), Google Maps platform.

High-level interaction summary:
- Frontend communicates with backend through REST/JSON.
- Backend manages business logic, validation, and integrations.
- MySQL stores relational transactional data.
- Firebase handles real-time communication, notifications, and document/file workflows.

Read more: [1. Design System Architecture/high-level architecture diagram .md](1.%20Design%20System%20Architecture/high-level%20architecture%20diagram%20.md)

---

## 3) Components, Classes, and Database Design

### Frontend Components
Main UI modules include:
- Public Home Page
- Authentication Screens
- Service Request Wizard
- Companion Selection
- Payment Screen
- Trip Tracking and Chat
- Companion Dashboard/Profile/Wallet
- Ratings, Support, and Admin Review screens

Read more: [2. Define Components, Classes, and Database Design/UI components and interactions.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/UI%20components%20and%20interactions.md)

### Backend Classes and Domain Model
The backend defines core domain entities for:
- identity/authentication (`User`, `UserConsent`, `OTPVerification`),
- companion operations (`CompanionProfile`, `CompanionDocument`, `CompanionAvailability`),
- request lifecycle (`ServiceRequest`, `RequestBeneficiary`, `RequestLocation`, `TripSession`),
- payment/wallet (`Payment`, `Refund`, `Wallet`, `WithdrawalRequest`),
- quality/support/admin (`Rating`, `SupportTicket`, `AdminAction`, `AuditLog`),
- real-time layer (`Chat`, `ChatMessage`, `LiveTracking`, `Notification`).

Read more: [2. Define Components, Classes, and Database Design/Backend.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Backend.md)

### Database Design (SQL + Firestore)
- **MySQL schema** covers relational entities, FK relationships, and an ER diagram.
- **Firestore collections** cover real-time chat, tracking, presence, notifications, and support chat.

Read more (MySQL): [2. Define Components, Classes, and Database Design/Databases/mysql schema.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Databases/mysql%20schema.md)

Read more (Firebase): [2. Define Components, Classes, and Database Design/Databases/firebase schema.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Databases/firebase%20schema.md)

---

## 4) Sequence Diagrams

Key high-level interaction flows are documented for:
1. Service request creation.
2. Companion acceptance and user notification.
3. Real-time trip tracking updates.

These diagrams show end-to-end interactions between frontend, backend, Firebase Auth/Firestore, and MySQL.

Read more: [3. Create High-Level Sequence Diagrams/High-Level Sequence Diagrams.md](3.%20Create%20High-Level%20Sequence%20Diagrams/High-Level%20Sequence%20Diagrams.md)

---

## 5) API Specifications

### External APIs
- Moyasar Payments API
- Firebase Firestore / Realtime API
- Firebase Storage API
- Google Maps Platform API

### Internal APIs
Internal endpoints are organized by domain:
- Auth
- Users
- Companions
- Requests
- Payments
- Trips
- Ratings
- Support
- Admin

Each endpoint group documents input/output shape and expected behavior.

Read more: [4. Document External and Internal APIs/External and Internal APIs.md](4.%20Document%20External%20and%20Internal%20APIs/External%20and%20Internal%20APIs.md)

---

## 6) SCM and QA Plans

### SCM Strategy
- Git-based workflow with protected branches.
- Branch model: `main`, `development`, `feature/*`, `hotfix/*`.
- PR-first workflow with code review and required checks.

### QA Strategy
- Shared team responsibility for testing.
- Unit tests + integration tests + manual critical-flow validation.
- Tooling: Jest and Postman.
- Staging and production quality gates in deployment pipeline.

Read more: [5. Plan SCM and QA Strategies/SCM and QA Strategies.md](5.%20Plan%20SCM%20and%20QA%20Strategies/SCM%20and%20QA%20Strategies.md)

---

## 7) Technical Justifications

### Why these technologies were selected
- **React**: suitable SPA architecture for role-based interfaces and dynamic flows.
- **Node.js + Express**: efficient REST API layer and strong ecosystem support.
- **MySQL**: strong relational consistency for transactional business data.
- **Firebase Firestore**: real-time communication needs (chat, live tracking, notifications).
- **Firebase Storage**: secure and scalable file/document handling.
- **Firebase Authentication**: token-based authentication integration.
- **Moyasar**: local payment support and webhook/refund workflows.
- **Google Maps Platform**: mapping, geocoding, and trip visualization.

### Why this architecture/design
- Separation of concerns across client/server/data layers.
- Mixed SQL + NoSQL design to balance transactional integrity with real-time performance.
- API-first backend with modular domains for maintainability and future scaling.
- QA gates and SCM controls to reduce regression risk and improve release quality.

---

## Stage 3 Source Index

- [0. Define User Stories and Mockups/User Stories.md](0.%20Define%20User%20Stories%20and%20Mockups/User%20Stories.md)
- [0. Define User Stories and Mockups/Mockups.md](0.%20Define%20User%20Stories%20and%20Mockups/Mockups.md)
- [1. Design System Architecture/high-level architecture diagram .md](1.%20Design%20System%20Architecture/high-level%20architecture%20diagram%20.md)
- [2. Define Components, Classes, and Database Design/UI components and interactions.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/UI%20components%20and%20interactions.md)
- [2. Define Components, Classes, and Database Design/Backend.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Backend.md)
- [2. Define Components, Classes, and Database Design/Databases/mysql schema.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Databases/mysql%20schema.md)
- [2. Define Components, Classes, and Database Design/Databases/firebase schema.md](2.%20Define%20Components,%20Classes,%20and%20Database%20Design/Databases/firebase%20schema.md)
- [3. Create High-Level Sequence Diagrams/High-Level Sequence Diagrams.md](3.%20Create%20High-Level%20Sequence%20Diagrams/High-Level%20Sequence%20Diagrams.md)
- [4. Document External and Internal APIs/External and Internal APIs.md](4.%20Document%20External%20and%20Internal%20APIs/External%20and%20Internal%20APIs.md)
- [5. Plan SCM and QA Strategies/SCM and QA Strategies.md](5.%20Plan%20SCM%20and%20QA%20Strategies/SCM%20and%20QA%20Strategies.md)
