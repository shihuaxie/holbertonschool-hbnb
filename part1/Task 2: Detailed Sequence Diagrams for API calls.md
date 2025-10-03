### 🖥️ **Sequence Diagrams for API calls**
---
- **Purpose**: 
  - The following 4 sequence diagrams illustrate how different components of the Hbnb application interact when handling user requests. Each diagram shows the **flow of information** across the `Presentation Layer` (API/controllers), `Business Logic Layer` (facades/services), and `Persistence Layer` (repositories/database).
  - The goal is to make the **request/response** lifecycle transparent, showing both the **happy path** (successful case) and **important error** branches (e.g., validation errors, not found, conflicts).
---
- **Included API Calls**
  - 1️⃣ **User Registration** (`POST /users`)
	- *What it does*: Allows a new user to sign up with `email`, `password`, and `personal details`.
	- *Flow*: User submits form → API validates → Business Logic checks email uniqueness → Repository inserts new record → DB saves → Response returns new UserDto.
	- *Error Cases*: Invalid payload (400), email already exists (409).
  - 2️⃣ **Place Creation** (`POST /places`)
	- *What it does*: Lets an authenticated user create a new rental property listing.
	- *Flow*: User calls API with place data → API forwards to Business Logic → BL opens transaction, saves place → attaches amenities via Repository → DB commit → Response returns PlaceDto.
	- *Error Cases*: Invalid data (400), not allowed (403), DB failure → rollback (500).
  - 3️⃣ **Review Submission** (`POST /places/{id}/reviews`)
	- *What it does*: Allows a user to leave a review for a place, including rating and comment.
	- *Flow*: User submits review → API forwards to BL → BL checks if place exists → Repository inserts review → DB saves → Response returns ReviewDto.
	- *Error Cases*: Place not found (404), rating out of range (400).
  - 4️⃣ **Fetching a List of Places** (`GET /places`)
	- *What it does*: Returns a paginated list of places, optionally filtered by price or amenities.
	- *Flow*: User sends query → API parses filters → BL forwards query to Repository → Repository builds SQL with joins → DB returns results → BL maps to PlaceSummaryDto → Response returns list.
	- *Error Cases*: No results (200 with empty list), invalid filters (400).
---
- **Key Roles of Sequence Diagrams**
  - **Presentation Layer**: Handles request/response, validates inputs, maps errors to HTTP codes.
  - **Business Logic Layer**: Implements rules, orchestrates transactions, and ensures data integrity.
  - **Persistence Layer**: Encapsulates DB access, executes queries, and returns entities to BL.
---
- **Why These 4 APIs?**
- **These APIs represent the core functionality of an Airbnb-like system:**
  - **Register users** → build the user base
  - **Create places** → supply side of rentals
  - **Add reviews** → social proof and quality control
  - **Fetch places** → demand side for users searching listings
    
<img width="1941" height="1246" alt="mermaid-diagram-2025-10-03-160757" src="https://github.com/user-attachments/assets/137ceee5-b1fb-436f-9ba9-ac9ec3143a37" />
<img width="1941" height="1246" alt="mermaid-diagram-2025-10-03-160834" src="https://github.com/user-attachments/assets/655eddeb-2816-4bd9-94d7-3ac83ef542ea" />
