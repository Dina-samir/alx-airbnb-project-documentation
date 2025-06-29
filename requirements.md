# 📋 Technical and Functional Requirements

This document outlines the backend requirements for core features of the **Airbnb Clone** platform. It covers functional goals, API specifications, validation rules, and performance expectations.

---

## 🔐 1. User Authentication

### ✅ Functional Requirements:
- Users (guests or hosts) can register, log in, and manage their profile securely.
- Authentication handled via JWT tokens.
- OAuth integration (e.g., Google, Facebook) optional.

### 🔧 API Endpoints:

| Method | Endpoint          | Description           |
|--------|-------------------|-----------------------|
| POST   | /api/register     | Register new account  |
| POST   | /api/login        | Login and get token   |
| GET    | /api/profile      | Get user profile      |
| PUT    | /api/profile      | Update user profile   |

### 📥 Input/Output Example:

**POST /api/register**
```json
Request:
{
  "email": "user@example.com",
  "password": "securepassword",
  "role": "guest"
}

Response:
{
  "message": "User registered successfully.",
  "token": "jwt-token-here"
}
```

### ✅ Validation Rules:
- Email must be valid and unique.
- Password: min 8 chars, 1 capital letter, 1 special char.
- Role must be guest or host.

### 🚀 Performance Criteria:
- Response time < 300ms for login/register.
- Token generation under 100ms.

------------------------------------------------------------------------


## 🏠 2. Property Management

### ✅ Functional Requirements:
- Hosts can create, update, and delete property listings.
- Listings include title, description, price, location, amenities, availability, and images.

### 🔧 API Endpoints:
- Method	Endpoint	Description
POST	/api/listings	Create a new listing
GET	/api/listings	Get all listings (with filters)
GET	/api/listings/:id	Get specific listing details
PUT	/api/listings/:id	Update listing
DELETE	/api/listings/:id	Delete listing

| Method | Endpoint            | Description                       |
|--------|-------------------  |-----------------------------------|
| POST   | /api/listings       | Create a new listing              |
| GET    | /api/listings       | Get all listings (with filters)   |
| GET    | /api/listings/:id   | Get specific listing details      |
| PUT    | /api/listings/:id   | Update listing                    |
| DELETE | /api/listings/:id   | Delete listing                    |

### 📥 Input/Output Example:

**POST /api/listings**
```json
Request:
{
  "title": "Modern Loft",
  "location": "Cairo, Egypt",
  "price": 120,
  "description": "Spacious loft in downtown",
  "amenities": ["WiFi", "AC", "Kitchen"],
  "availability": ["2025-07-01", "2025-07-10"]
}

Response:
{
  "message": "Listing created successfully",
  "listing_id": 103
}
```

### ✅ Validation Rules:
- Price must be numeric and > 0.
- Availability must include valid ISO dates.
- Title and description are required.

### 🚀 Performance Criteria:
- Listing creation in < 500ms.
- Filters should respond in < 1s for up to 10k listings.

------------------------------------------------------------------------

## 📆 3. Booking System

### ✅ Functional Requirements:
- Guests can book properties by selecting dates.
- The system checks availability and prevents double bookings.
- Statuses: pending, confirmed, cancelled, completed.

### 🔧 API Endpoints:
|Method	 |Endpoint	Description
|--------|-------------------  |-----------------------------------|
|POST	 |/api/bookings	       |Create a new booking
|GET	 |/api/bookings/:id	   |Get booking details
|PUT	 |/api/bookings/:id	   |Cancel or update booking

### 📥 Input/Output Example:

**POST /api/bookings**

```json
Request:
{
  "property_id": 103,
  "start_date": "2025-07-03",
  "end_date": "2025-07-07"
}

Response:
{
  "message": "Booking successful",
  "booking_id": 45,
  "status": "confirmed"
}
```

### ✅ Validation Rules:
- Start date must be before end date.
- Dates must not overlap existing confirmed bookings.
- Authenticated guest only can create a booking.

### 🚀 Performance Criteria:
- Booking creation in < 700ms.
- Conflict checking and insertion < 500ms.