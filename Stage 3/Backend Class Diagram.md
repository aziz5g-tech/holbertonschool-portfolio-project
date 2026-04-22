```mermaid
classDiagram

%% =====================
%% Controllers Layer
%% =====================

class UserController {
  +register()
  +login()
  +getProfile()
  +updateProfile()
}

class ServiceRequestController {
  +createRequest()
  +getRequestById()
  +getUserRequests()
  +updateRequestStatus()
  +cancelRequest()
}

class CompanionController {
  +getProfile()
  +getAvailableCompanions()
  +acceptRequest()
  +rateCompanion()
}

class MapController {
  +setDestination()
}

class ContentController {
  +getEducationalContent()
}

class FavoriteController {
  +addFavorite()
  +removeFavorite()
  +getFavorites()
}

%% =====================
%% Services Layer
%% =====================

class AuthService {
  +verifyToken()
  +getUserFromToken()
}

class RequestService {
  +createServiceRequest()
  +assignCompanion()
  +updateStatus()
}

class CompanionService {
  +fetchProfile()
  +setAvailability()
  +rateCompanion()
}

class NotificationService {
  +sendRealtimeUpdate()
  +notifyServiceSeeker()
  +notifyElderlyUser()
}

class UserService {
  +createUser()
  +updateUser()
  +fetchUser()
}

%% =====================
%% Data Layer
%% =====================

class MySQLRepository {
  +insert()
  +update()
  +find()
  +delete()
}

class FirebaseAuthService {
  +verifyToken()
}

class FirebaseRealtimeService {
  +sendRealtimeUpdate()
  +storeNotification()
}

%% =====================
%% Relationships
%% =====================

UserController --> AuthService
UserController --> UserService

ServiceRequestController --> RequestService
CompanionController --> CompanionService
MapController --> RequestService
FavoriteController --> MySQLRepository
ContentController --> MySQLRepository

RequestService --> MySQLRepository
UserService --> MySQLRepository
CompanionService --> MySQLRepository

AuthService --> FirebaseAuthService
NotificationService --> FirebaseRealtimeService
```
