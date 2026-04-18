# System Architecture Diagram - المرافق (Al-Murafeq)

## 1️⃣ High-Level Package Diagram (Three-Layer Architecture)

```mermaid
graph TB
    subgraph "Presentation Layer"
        WEB["🌐 Web Client<br/>React.js"]
        MOBILE["📱 Mobile Client<br/>React Native"]
    end

    subgraph "Business Logic Layer (Facade Pattern)"
        FACADE["🎭 API Facade<br/>Controllers & Routers"]
        
        subgraph "Services"
            USERSERVICE["👤 User Service"]
            PLACESERVICE["📍 Place Service"]
            REVIEWSERVICE["⭐ Review Service"]
            AUTHSERVICE["🔐 Auth Service"]
        end

        subgraph "Business Objects"
            USEROBJ["User Entity"]
            PLACEOBJ["Place Entity"]
            REVIEWOBJ["Review Entity"]
            AMENITYOBJ["Amenity Entity"]
        end

        FACADE -->|Route Requests| USERSERVICE
        FACADE -->|Route Requests| PLACESERVICE
        FACADE -->|Route Requests| REVIEWSERVICE
        FACADE -->|Route Requests| AUTHSERVICE

        USERSERVICE -->|Manage| USEROBJ
        PLACESERVICE -->|Manage| PLACEOBJ
        REVIEWSERVICE -->|Manage| REVIEWOBJ
        PLACEOBJ -->|Contains| AMENITYOBJ
    end

    subgraph "Data Access Layer"
        USERREPO["User Repository"]
        PLACEREPO["Place Repository"]
        REVIEWREPO["Review Repository"]
        AMENITYREPO["Amenity Repository"]

        subgraph "Database"
            DB["🗄️ PostgreSQL Database"]
        end

        USERREPO -->|Query/Update| DB
        PLACEREPO -->|Query/Update| DB
        REVIEWREPO -->|Query/Update| DB
        AMENITYREPO -->|Query/Update| DB
    end

    WEB -->|HTTP/REST| FACADE
    MOBILE -->|HTTP/REST| FACADE

    USERSERVICE -->|Use| USERREPO
    PLACESERVICE -->|Use| PLACEREPO
    PLACESERVICE -->|Use| AMENITYREPO
    REVIEWSERVICE -->|Use| REVIEWREPO

    style WEB fill:#4A90E2,color:#fff
    style MOBILE fill:#4A90E2,color:#fff
    style FACADE fill:#F5A623,color:#fff
    style USERSERVICE fill:#7ED321
    style PLACESERVICE fill:#7ED321
    style REVIEWSERVICE fill:#7ED321
    style AUTHSERVICE fill:#7ED321
    style USEROBJ fill:#50E3C2,color:#fff
    style PLACEOBJ fill:#50E3C2,color:#fff
    style REVIEWOBJ fill:#50E3C2,color:#fff
    style AMENITYOBJ fill:#50E3C2,color:#fff
    style USERREPO fill:#BD10E0,color:#fff
    style PLACEREPO fill:#BD10E0,color:#fff
    style REVIEWREPO fill:#BD10E0,color:#fff
    style AMENITYREPO fill:#BD10E0,color:#fff
    style DB fill:#FF6B6B,color:#fff
```

---

## 2️⃣ Detailed Class Diagram for Business Logic Layer

```mermaid
classDiagram
    class User {
        -int userId
        -string name
        -string email
        -string password
        -string phoneNumber
        -string userType
        -text profileImage
        -text bio
        -double avgRating
        -datetime createdAt
        -datetime updatedAt
        
        +register() boolean
        +login() string
        +updateProfile() boolean
        +getProfile() User
        +uploadProfileImage() boolean
        +getReviews() Review[]
        +getFavorites() Place[]
        +addFavorite(placeId) boolean
        +removeFavorite(placeId) boolean
        +getRating() double
    }

    class Place {
        -int placeId
        -int ownerId
        -string name
        -string description
        -string category
        -double latitude
        -double longitude
        -string address
        -string city
        -double avgRating
        -datetime createdAt
        -datetime updatedAt
        
        +createPlace() boolean
        +updatePlace() boolean
        +deletePlace() boolean
        +getDetails() Place
        +getLocation() Location
        +getAmenities() Amenity[]
        +addAmenity(amenityId) boolean
        +removeAmenity(amenityId) boolean
        +getReviews() Review[]
        +getRating() double
        +search(query) Place[]
    }

    class Amenity {
        -int amenityId
        -string name
        -string icon
        -string description
        -datetime createdAt
        
        +create() boolean
        +update() boolean
        +delete() boolean
        +getDetails() Amenity
        +getName() string
        +getIcon() string
    }

    class Review {
        -int reviewId
        -int placeId
        -int userId
        -int rating
        -string title
        -text comment
        -int helpfulCount
        -datetime createdAt
        -datetime updatedAt
        
        +submitReview() boolean
        +updateReview() boolean
        +deleteReview() boolean
        +getDetails() Review
        +getRating() int
        +getComment() string
        +markHelpful() boolean
        +getAuthorInfo() User
    }

    class Location {
        -double latitude
        -double longitude
        -string address
        -string city
        -string zipCode
        
        +calculateDistance(otherLocation) double
        +isValid() boolean
        +getCoordinates() tuple
    }

    Place "*" --> "1" User : "ownedBy"
    Place "1" --> "*" Amenity : "hasMany"
    Review "1" --> "1" Place : "reviewsOf"
    Review "1" --> "1" User : "writtenBy"
    Place "1" --> "1" Location : "locatedAt"
    User "1" --> "*" Place : "hasFavorites"
```

---

## 3️⃣ Sequence Diagrams for API Calls

### 3.1 User Registration API Call

```mermaid
sequenceDiagram
    participant Client as 📱 Client
    participant Controller as 🎭 Controller<br/>(Facade)
    participant UserService as 👤 UserService
    participant UserRepo as 📦 UserRepository
    participant DB as 🗄️ Database

    Client->>Controller: POST /api/users/register<br/>{name, email, password}
    activate Controller
    
    Controller->>UserService: register(userData)
    activate UserService
    
    UserService->>UserService: validateInput()
    alt Validation Failed
        UserService-->>Controller: throw ValidationError
        Controller-->>Client: 400 Bad Request
    end
    
    UserService->>UserService: hashPassword()
    UserService->>UserRepo: createUser(userData)
    activate UserRepo
    
    UserRepo->>DB: INSERT INTO users<br/>VALUES(...)
    DB-->>UserRepo: userId, createdAt
    deactivate UserRepo
    
    UserRepo-->>UserService: User Object
    UserService->>UserService: generateJWT()
    UserService-->>Controller: {token, user}
    deactivate UserService
    
    Controller-->>Client: 201 Created<br/>{token, user}
    deactivate Controller
```

### 3.2 Create Place API Call

```mermaid
sequenceDiagram
    participant Client as 📱 Client
    participant Controller as 🎭 Controller<br/>(Facade)
    participant AuthService as 🔐 AuthService
    participant PlaceService as 📍 PlaceService
    participant AmenityRepo as 📦 AmenityRepository
    participant PlaceRepo as 📦 PlaceRepository
    participant DB as 🗄️ Database

    Client->>Controller: POST /api/places<br/>{name, location, amenities[]}<br/>Header: Authorization
    activate Controller
    
    Controller->>AuthService: validateToken(token)
    activate AuthService
    AuthService-->>Controller: userId
    deactivate AuthService
    
    Controller->>PlaceService: createPlace(placeData, userId)
    activate PlaceService
    
    PlaceService->>PlaceService: validateLocation()
    PlaceService->>PlaceRepo: createPlace(placeData)
    activate PlaceRepo
    
    PlaceRepo->>DB: INSERT INTO places<br/>VALUES(...)
    DB-->>PlaceRepo: placeId
    deactivate PlaceRepo
    
    loop For Each Amenity
        PlaceService->>AmenityRepo: getAmenity(amenityId)
        activate AmenityRepo
        AmenityRepo->>DB: SELECT * FROM amenities
        DB-->>AmenityRepo: Amenity Object
        deactivate AmenityRepo
        
        PlaceService->>DB: INSERT INTO place_amenities<br/>VALUES(placeId, amenityId)
    end
    
    PlaceService-->>Controller: Place Object with Amenities
    deactivate PlaceService
    
    Controller-->>Client: 201 Created<br/>{placeId, name, amenities}
    deactivate Controller
```

### 3.3 Submit Review API Call

```mermaid
sequenceDiagram
    participant Client as 📱 Client
    participant Controller as 🎭 Controller<br/>(Facade)
    participant AuthService as 🔐 AuthService
    participant ReviewService as ⭐ ReviewService
    participant PlaceRepo as 📦 PlaceRepository
    participant ReviewRepo as 📦 ReviewRepository
    participant DB as 🗄️ Database

    Client->>Controller: POST /api/reviews<br/>{placeId, rating, comment}<br/>Header: Authorization
    activate Controller
    
    Controller->>AuthService: validateToken(token)
    activate AuthService
    AuthService-->>Controller: userId
    deactivate AuthService
    
    Controller->>ReviewService: submitReview(reviewData, userId)
    activate ReviewService
    
    ReviewService->>ReviewService: validateRating(1-5)
    alt Invalid Rating
        ReviewService-->>Controller: throw ValidationError
        Controller-->>Client: 400 Bad Request
    end
    
    ReviewService->>PlaceRepo: getPlace(placeId)
    activate PlaceRepo
    PlaceRepo->>DB: SELECT * FROM places
    DB-->>PlaceRepo: Place Object
    deactivate PlaceRepo
    
    ReviewService->>ReviewService: checkDuplicate(placeId, userId)
    
    ReviewService->>ReviewRepo: createReview(reviewData)
    activate ReviewRepo
    ReviewRepo->>DB: INSERT INTO reviews<br/>VALUES(...)
    DB-->>ReviewRepo: reviewId
    deactivate ReviewRepo
    
    ReviewService->>ReviewService: updatePlaceRating(placeId)
    ReviewService->>DB: UPDATE places<br/>SET avgRating = CALC_AVG()
    
    ReviewService-->>Controller: Review Object
    deactivate ReviewService
    
    Controller-->>Client: 201 Created<br/>{reviewId, rating, comment}
    deactivate Controller
```

### 3.4 Fetch Places List API Call

```mermaid
sequenceDiagram
    participant Client as 📱 Client
    participant Controller as 🎭 Controller<br/>(Facade)
    participant PlaceService as 📍 PlaceService
    participant PlaceRepo as 📦 PlaceRepository
    participant AmenityRepo as 📦 AmenityRepository
    participant DB as 🗄️ Database
    participant Cache as ⚡ Cache (Redis)

    Client->>Controller: GET /api/places<br/>?city=Riyadh&limit=20&offset=0
    activate Controller
    
    Controller->>PlaceService: getPlaces(filters, pagination)
    activate PlaceService
    
    alt Check Cache
        PlaceService->>Cache: getCachedPlaces(cacheKey)
        alt Cache Hit
            Cache-->>PlaceService: places[]
        else Cache Miss
            PlaceService->>PlaceRepo: searchPlaces(filters, limit, offset)
            activate PlaceRepo
            
            PlaceRepo->>DB: SELECT * FROM places<br/>WHERE city = 'Riyadh'<br/>LIMIT 20 OFFSET 0
            DB-->>PlaceRepo: places[]
            deactivate PlaceRepo
            
            loop For Each Place
                PlaceService->>AmenityRepo: getAmenities(placeId)
                activate AmenityRepo
                AmenityRepo->>DB: SELECT * FROM place_amenities<br/>WHERE placeId = ?
                DB-->>AmenityRepo: amenities[]
                deactivate AmenityRepo
            end
            
            PlaceService->>Cache: setCachedPlaces(cacheKey, places)
        end
    end
    
    PlaceService-->>Controller: places[] with Amenities
    deactivate PlaceService
    
    Controller-->>Client: 200 OK<br/>{places[], totalCount, page}
    deactivate Controller
```

---

## 📋 Summary of Relationships

| Relationship | Type | Cardinality |
|-------------|------|-------------|
| User owns Place | One-to-Many | 1 User → * Places |
| Place has Amenity | Many-to-Many | * Places ↔ * Amenities |
| User writes Review | One-to-Many | 1 User → * Reviews |
| Place receives Review | One-to-Many | 1 Place → * Reviews |
| Place has Location | One-to-One | 1 Place → 1 Location |
| User has Favorites | Many-to-Many | * Users ↔ * Places |

---

## 🏗️ Architecture Layers Explanation

### **Presentation Layer**
- Handles user interface
- Communicates only with API Facade
- No direct database access

### **Business Logic Layer (Facade Pattern)**
- API Controllers act as a single entry point
- Services encapsulate business rules
- Business entities represent domain objects
- Repositories pattern for data access abstraction

### **Data Access Layer**
- Repositories handle all database queries
- Single responsibility: data persistence
- Decoupled from business logic

