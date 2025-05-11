Here's the **markdown-formatted specification** for the three key backend features of the Airbnb Clone project:

---

# 🏗️ Airbnb Clone – Backend Feature Specifications

## 1️⃣ User Authentication

### ✅ Overview

Handles user registration, login, and secure session management using JWT.

### 🔗 API Endpoints

| Method | Endpoint             | Description                       |
| ------ | -------------------- | --------------------------------- |
| POST   | `/api/auth/register` | Register a new user               |
| POST   | `/api/auth/login`    | Authenticate user and return JWT  |
| GET    | `/api/auth/profile`  | Retrieve logged-in user's profile |
| PUT    | `/api/auth/profile`  | Update user profile information   |

### 📥 Input Example

#### `POST /api/auth/register`

```json
{
  "email": "user@example.com",
  "password": "securePass123",
  "role": "guest",
  "fullName": "John Doe"
}
```

### ✅ Validation Rules

* `email`: Must be a valid, unique email.
* `password`: Minimum 8 characters, alphanumeric.
* `role`: Must be `guest` or `host`.
* `fullName`: Required string.

### 📤 Output (Success)

```json
{
  "message": "User registered successfully",
  "token": "JWT_TOKEN_HERE"
}
```

### 🔒 Security & Performance

* Passwords hashed using bcrypt.
* JWT tokens expire in 24 hours.
* Login attempts are rate-limited (e.g., max 5 per minute per IP).

---

## 2️⃣ Property Management

### ✅ Overview

Allows hosts to create and manage rental property listings.

### 🔗 API Endpoints

| Method | Endpoint              | Description                    |
| ------ | --------------------- | ------------------------------ |
| POST   | `/api/properties`     | Create a new property listing  |
| GET    | `/api/properties`     | Get list of available listings |
| GET    | `/api/properties/:id` | Get detailed view of a listing |
| PUT    | `/api/properties/:id` | Update an existing listing     |
| DELETE | `/api/properties/:id` | Delete a property listing      |

### 📥 Input Example

#### `POST /api/properties`

```json
{
  "title": "Cozy Cabin in the Woods",
  "description": "A peaceful retreat near the lake.",
  "location": "Asheville, NC",
  "price": 150.00,
  "maxGuests": 4,
  "amenities": ["wifi", "kitchen", "parking"],
  "availability": [
    {"from": "2025-06-01", "to": "2025-06-10"}
  ]
}
```

### ✅ Validation Rules

* `title`, `description`: Required strings.
* `price`: Must be a positive number.
* `maxGuests`: Integer > 0.
* `amenities`: Must match allowed list (e.g., wifi, pool).
* `availability`: Dates must be in the future and not overlap.

### 📤 Output (Success)

```json
{
  "message": "Property created successfully",
  "propertyId": "abc123"
}
```

### ⚙️ Performance Criteria

* Pagination: Limit 20 listings per page.
* Redis cache for frequently viewed listings.
* Indexed fields: location, price, availability.

---

## 3️⃣ Booking System

### ✅ Overview

Guests can book properties for specific dates, and hosts can manage bookings.

### 🔗 API Endpoints

| Method | Endpoint            | Description                   |
| ------ | ------------------- | ----------------------------- |
| POST   | `/api/bookings`     | Create a new booking          |
| GET    | `/api/bookings`     | Get bookings for current user |
| PUT    | `/api/bookings/:id` | Cancel or update a booking    |

### 📥 Input Example

#### `POST /api/bookings`

```json
{
  "propertyId": "abc123",
  "checkIn": "2025-06-01",
  "checkOut": "2025-06-05"
}
```

### ✅ Validation Rules

* `checkIn` and `checkOut`: Must be valid future dates.
* Dates must not overlap existing bookings.
* `propertyId` must be valid and available.

### 📤 Output (Success)

```json
{
  "message": "Booking confirmed",
  "bookingId": "book789",
  "status": "confirmed"
}
```

### ⚙️ Performance Criteria

* Use transactions to prevent double bookings.
* Redis can cache availability data.
* Query optimization using indexes on date fields.

