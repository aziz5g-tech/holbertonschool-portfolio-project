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

# Back-end Components (Classes) : 

## User (Base Entity):
**Attributes:**
  - id: int (PK)
  - password: string
  - name: string
  - email: string
  - mobileNumber: string
  - birthdate: date
  - city: string
  - district: string
  - role: enum('seeker', 'provider', 'admin')
  - createdAt: datetime 
**Methods:**
  - register()
  - login()
  - updateProfile()

## UserLanguages:
**Attributes:**
  - id: int (PK)
  - user_id: int (FK → User.id)
  - language: string 
**Methods:**
  - addLanguage()
  - deleteLanguage()
  - getUserLanguages()

## UserFiles
**Attributes:**
  - id: int (PK)
  - user_id: int (FK → User.id)
  - file_url: string
  - file_type: enum('id_card', 'certification', 'profile_picture')
  - created_at: datetime 
**Methods:**
  - uploadFile()
  - deleteFile()
  - getUserFiles()

## Provider (Profile):
**Attributes:**
  - user_id: int (FK → User.id)
  - experience: boolean
  - bio: string
  - hasCar: boolean
  - carType: string
  - iban: string
  - isHealthy: boolean 
**Methods:**
  - updateAvailability()
  - deleteService()
  - submitOffer(requestId)

## providerServices
**Attributes:**
  - id: int (PK)
  - provider_id: int (FK → Provider.user_id)
  - serviceName: string 
**Methods:**
  - addService()
  - deleteService()
  - getProviderServices(providerId)
  
## ProviderAvailability
**Attributes:**
  - id: int (PK)
  - provider_id: int (FK → Provider.user_id)
  - day: string
  - time_from: time
  - time_to: time 
**Methods:**
  - setAvailability()
  - deleteAvailability()
  - updateAvailability()
  - getAvailability(providerId)
  
## Seeker (Profile):
**Attributes:**
  - user_id: int  (FK → User.id)
  - emergencyNumber: string
  - notes: string
  - requiresWheelchair: boolean
  - preferredGender: enum('male', 'female', 'any') 
**Methods:**
  - createRequest()
  - cancelRequest()
  - viewOffers(requestId)
  - acceptOffer(offerId)
  - rejectOffer(offerId)
  
## Rating :
**Attributes:**
  - id: int
  - user_id: int  (FK → User.id)
  - target_id: int (FK → User.id)
  - rating: int (1–5)
  - comment: String
  - created_at: Datetime 
**Methods:**
  - submitRating()
  - getUserRatings(userId)
  - deleteRating()
  - updateRating()

