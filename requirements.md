# Airbnb Clone Backend Requirement Specifications

This document outlines the detailed technical and functional requirements for key backend features of the Airbnb Clone application.

---

## 1. User Authentication

### 🔐 Feature Description

Enable secure user registration, login, and session management using **JWT (JSON Web Token)**.

### 🔁 Endpoints

- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/profile`

### 📥 Input Example (Register)

```json
{
  "first_name": "Jane",
  "last_name": "Smith",
  "email": "jane@example.com",
  "password": "StrongPass123!"
}
```

### 📥 Input Example (Login)

```json
{
  "email": "jane@example.com",
  "password": "StrongPass123!"
}
```

### 📤 Output Example (Success)

```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "uuid",
    "first_name": "Jane",
    "email": "jane@example.com"
  }
}
```

### 🧪 Validation

- Email must be valid and unique
- Password must be at least 8 characters
- No empty fields allowed

### ⚙️ Performance

- Token generation < 100ms
- Password hashing using bcrypt (10-12 rounds)

---

## 2. Property Management

### 🏠 Feature Description

Hosts can create, edit, delete, and view their property listings.

### 🔁 Endpoints

- `POST /api/properties`
- `GET /api/properties`
- `GET /api/properties/:id`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`

### 📥 Input Example (Create)

```json
{
  "title": "Cozy Apartment",
  "description": "2-bedroom flat with a sea view",
  "location": "Barcelona",
  "price": 90,
  "amenities": ["Wi-Fi", "Air Conditioning"]
}
```

### 📤 Output Example (Success)

```json
{
  "id": "uuid",
  "title": "Cozy Apartment",
  "host_id": "uuid"
}
```

### 🧪 Validation

- All fields are required
- Price must be numeric and > 0
- Title and location should be strings under 100 characters

### ⚙️ Performance

- Listings fetch should return results in < 300ms
- Indexing applied on location and price fields

---

## 3. Booking System

### 📅 Feature Description

Allow guests to book available properties for specific dates and track their status.

### 🔁 Endpoints

- `POST /api/bookings`
- `GET /api/bookings`
- `GET /api/bookings/:id`
- `DELETE /api/bookings/:id`

### 📥 Input Example (Booking)

```json
{
  "property_id": "uuid",
  "start_date": "2025-08-01",
  "end_date": "2025-08-05"
}
```

### 📤 Output Example

```json
{
  "id": "uuid",
  "status": "pending",
  "total_price": 450
}
```

### 🧪 Validation

- Dates must be valid and not overlap with existing bookings
- Cannot book a property owned by the user
- Dates must be in the future

### ⚙️ Performance

- Booking availability check in < 200ms
- Auto-cancel if payment is not received within 15 minutes (queue-based)
