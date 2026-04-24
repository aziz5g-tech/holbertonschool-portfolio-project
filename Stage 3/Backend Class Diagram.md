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

###

### Back-end Components (Classes) :
## User:
# Attributes:
- id: int
- name: String
- Email: String
- MobileNumber: int
- Birthdate: Date
- City: String
- District: String
- Languages: List<String>
- Rating
- role (Provider, Seeker)

## Provider:
# Attributes:
- User.id: int (FK)
- Services: List<String>
- OfferDays: List<DAY>
- OfferTime: List<Time>
- Experinces: Boolean
- Bio: String
- HaveCar: Boolean
- Car: String
- Storage: Links >>>>>>>>>>>>>>>>> ?? الملفات تنرفع وهنا ينحط رابط لها كيف تنكتب ؟
- Picture: نفس الشي
- Certifications: same
- IBAN: int
- Health: Boolean

## Seeker:  >>>>>> ?????????? وش الخاص فيه ؟
# Attributes:
- User.id: int (FK)
- EmergencyNumber: int
- ExtraInformation: String

## Admin:
- id: int
- 
