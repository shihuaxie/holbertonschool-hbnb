## Explanatory Notes

This detailed class diagram for the Business Logic layer illustrates the entities within this layer, their attributes, methods, and the relationships between them.

### **1.User**
- Represents users of the application (e.g.: a host, guest)
- **Attributes:**
    - `first_name`, `last_name`
    - `email`, `password`
    - `is_admin` (boolean flag for admin status)
    -  Inherits `id`, `created_at`, and `updated_at` from **BaseModel**
- **Methods:**
    - `register(data: dict)`: create a new user
    - `update_profile(data: dict)`: update user information
    - `delete()`: remove the user
    - `authenticate(email, password)`: validate credentials
  
### **2.Place**
- Represents rental listings listed by a user (owner)
- **Attributes:**
    - `title`, `description`, `price`
    - `latitude`, `longitude` (geolocation)
    - `owner_id` linking to the **User** who owns it
    - Inherits `id`, `created_at`, and `updated_at` from **BaseModel**
- **Methods:**
    - `create(data: dict)`, `update(data: dict)`, `delete()`
    - `list()`: retrieve all listings
    - `filter_by_owner(owner_id)`: retrieve listings by a specific user
    - `get_reviews()`: fetch reviews associated with this place
    - `get_average_rating()`: calculate the average rating from reviews

### **3.Amenity**
- Represents additional features of a `Place` (e.g. wi-fi, saunas)
- **Attributes:**
    - `name`, `description`
    - `place_id` linking to the corresponding **Place**
    - Inherits `id`, `created_at`, and `updated_at` from **BaseModel**
- **Methods:**
    - `create(data: dict)`, `update(data: dict)`, `delete()`
    - `list()`: retrieve all amenities
 
### **4.Review**
- Represents guest feedback on `Place`s
- **Attributes:**
    - `rating`, `comment`
    - `user_id` linking to the author and `place_id` linking to the **Place** reviewed
    - Inherits `id`, `created_at`, and `updated_at` from **BaseModel**
- **Methods:**
    - `create(data: dict)`, `update(data: dict)`, `delete()`
    - `list()`: retrieve all reviews
    - `get_average_rating(place_id)`: calculate average rating for a place

### **Relationships**
- **Aggregations:**
  - A `User` can **own multiple** `Place`s
  - A `Place` can **have multiple** `Review`s
  - A `User` can **write multiple** `Review`s
  - A `Place` can **include multiple** `Amenity`s
- **Dependencies:**
  - `User` depends on `Place` for methods that access owned listings
  - `Place` depends on `Review` to calculate ratings and fetch feedback
  - `Review` depends on `User` and `Place` to validate existence when creating or updating a review
  - `Amenity` depends on `Place` to ensure the corresponding place exists
- **Aggregations** indicate long-term structural ownership, while **Dependencies** indicate short-term, functional interactions
  
---
## Detailed Class Diagram for Business Logic Layer

``` mermaid
%%  HBnB Business Logic Layer UML
classDiagram

%% ===== Base Class =====
class BaseModel {
  +id: UUID4
  +created_at: datetime
  +updated_at: datetime
}
%% ===== User Class =====
class User {
  +is_admin: boolean
  +first_name: string
  +last_name: string
  +email: string
  +password: string

  +register(data: dict): User
  +update_profile(data: dict): void
  +delete(): boolean
  +authenticate(email: string, password: string): boolean
}
%% ===== Place Class =====
class Place {
  +owner_id: UUID4
  +title: string
  +description: string
  +price: float
  +latitude: float
  +longitude: float

  +create(data: dict): Place
  +update(data: dict): void
  +delete(): boolean
  +list(): List<Place>
  +filter_by_owner(owner_id: UUID4): List<Place>
  +get_reviews(): List<Review>
  +get_average_rating(): float
}
%% ===== Amenity Class =====
class Amenity {
  +place_id: UUID4
  +name: string
  +description: string

  +create(data: dict): Amenity
  +update(data: dict): void
  +delete(): boolean
  +list(): List<Amenity>
}
%% ===== Review Class =====
class Review {
  +place_id: UUID4
  +user_id: UUID4
  +rating: int
  +comment: string

  +create(data: dict): Review
  +update(data: dict): void
  +delete(): boolean
  +list(): List<Review>
  +get_average_rating(place_id: UUID4): float
}
%% ===== Inheritance =====
User --|> BaseModel
Place --|> BaseModel
Amenity --|> BaseModel
Review --|> BaseModel

%% ===== Associations =====
User "1" o-- "0..*" Place : aggregation >
Place "1" o-- "0..*" Review : aggregation >
User "1" o-- "0..*" Review : aggregation >
Place "1" o-- "0..*" Amenity : aggregation >

%% ===== Dependencies =====
User ..> Place : dependency >
Place ..> Review : dependency >
Place ..> Amenity : dependency >
Review ..> User : dependency >
Review ..> Place : dependency >
Amenity ..> Place : dependency >


%% ===== Styling =====
classDef entityStyle fill:#bbdefb,stroke:#1565c0,stroke-width:2px,color:#741b47,font-weight:bold;
classDef baseStyle fill:#fce4ec,stroke:#ad1457,stroke-width:2px,color:#880e4f,font-weight:bold;
```
