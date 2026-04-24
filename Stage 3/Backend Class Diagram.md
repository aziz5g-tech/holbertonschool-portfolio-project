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
  - createdAt: datetime<br>

**Methods:**
  - register()
  - login()
  - updateProfile()

## UserLanguages:
**Attributes:**
  - id: int (PK)
  - user_id: int (FK → User.id)
  - language: string<br>

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
  - created_at: datetime<br>

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
  - isHealthy: boolean<br>

**Methods:**
  - updateAvailability()
  - deleteService()
  - submitOffer(requestId)

## providerServices
**Attributes:**
  - id: int (PK)
  - provider_id: int (FK → Provider.user_id)
  - serviceName: string<br>

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
  - time_to: time<br>

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
  - healthStatus: String
  - requiresWheelchair: boolean
  - preferredGender: enum('male', 'female', 'any')<br>

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
  - created_at: Datetime<br>

**Methods:**
  - submitRating()
  - getUserRatings(userId)
  - deleteRating()
  - updateRating()















.
