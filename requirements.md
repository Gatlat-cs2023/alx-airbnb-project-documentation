# 📄 Backend Requirements Specification - Airbnb Backend System

This document outlines the **technical** and **functional** specifications for key backend features of the Airbnb backend system. It covers API endpoints, data formats, validation rules, and performance expectations.

---

## ✅ 1. User Authentication

### Objective
Allow users (guests, hosts, admins) to securely register, log in, and manage sessions.

### API Endpoints

| Method | Endpoint         | Description               |
|--------|------------------|---------------------------|
| POST   | /api/v1/signup   | Register a new user       |
| POST   | /api/v1/login    | Log in an existing user   |
| POST   | /api/v1/logout   | Log out the current user  |

### Input/Output Specification

- **POST /signup**
  - **Request Body:**
    ```json
    {
      "name": "John Doe",
      "email": "john@example.com",
      "password": "secret123",
      "role": "guest"
    }
    ```
  - **Response:**
    ```json
    {
      "message": "User registered successfully.",
      "user_id": "uuid"
    }
    ```

- **POST /login**
  - **Request Body:**
    ```json
    {
      "email": "john@example.com",
      "password": "secret123"
    }
    ```
  - **Response:**
    ```json
    {
      "token": "jwt-token",
      "expires_in": 3600
    }
    ```

### Validation Rules
- Email must be unique and valid format.
- Password minimum 8 characters, at least 1 number & 1 capital letter.
- Role must be one of: guest, host, admin.

### Performance Criteria
- Authentication responses < 500ms.
- Passwords must be hashed using bcrypt or equivalent.

---

## 🏠 2. Property Management

### Objective
Enable hosts to list, update, and delete properties. Allow admin moderation.

### API Endpoints

| Method | Endpoint             | Description                      |
|--------|----------------------|----------------------------------|
| POST   | /api/v1/properties   | Host adds new property listing   |
| GET    | /api/v1/properties   | Get all approved properties      |
| PATCH  | /api/v1/properties/:id | Update property details        |
| DELETE | /api/v1/properties/:id | Delete a property               |
| PATCH  | /api/v1/properties/:id/status | Admin approves/rejects listing |

### Input/Output Specification

- **POST /properties**
  - **Request Body:**
    ```json
    {
      "title": "Cozy Apartment",
      "description": "Close to beach",
      "price_per_night": 75,
      "location": "Addis Ababa",
      "amenities": ["wifi", "kitchen"],
      "images": ["url1", "url2"]
    }
    ```

  - **Response:**
    ```json
    {
      "message": "Property submitted for review.",
      "property_id": "uuid"
    }
    ```

### Validation Rules
- Price must be numeric and greater than 0.
- Title and description required.
- Host must be authenticated.
- Admin can approve/reject before visibility.

### Performance Criteria
- Listing creation < 800ms.
- Admin review queue supports pagination and filters.

---

## 📅 3. Booking System

### Objective
Enable guests to book available properties and hosts to view/manage bookings.

### API Endpoints

| Method | Endpoint                 | Description                  |
|--------|--------------------------|------------------------------|
| POST   | /api/v1/bookings         | Create a new booking         |
| GET    | /api/v1/bookings/guest   | Get all bookings for guest   |
| GET    | /api/v1/bookings/host    | Get all bookings for host    |
| PATCH  | /api/v1/bookings/:id     | Update (cancel) a booking    |

### Input/Output Specification

- **POST /bookings**
  - **Request Body:**
    ```json
    {
      "property_id": "uuid",
      "check_in": "2025-07-01",
      "check_out": "2025-07-07",
      "guests": 2
    }
    ```
  - **Response:**
    ```json
    {
      "message": "Booking confirmed.",
      "booking_id": "uuid"
    }
    ```

### Validation Rules
- Dates must be valid and `check_out > check_in`.
- Guests must not exceed property's allowed capacity.
- Property must be available during the selected dates.
- Payment must be confirmed (via integration).

### Performance Criteria
- Booking confirmation < 1000ms.
- Prevent double bookings via lock or atomic transactions.

---

## 🧩 Notes

- All endpoints should be secured using token-based (JWT) authentication.
- Use RESTful principles for consistency and scalability.
- Use PostgreSQL or MySQL for relational data storage.
- Sensitive information (e.g., passwords, tokens) must not be logged.

---

## 📂 File Location

- **Filename:** `requirements.md`  
- **Path:** Root directory of `alx-airbnb-project-documentation`

---

## ✍️ Author

Created for the **ALX Airbnb Project** under the **Backend Specialization Program**.  
All diagrams and documentation follow industry-standard conventions.

