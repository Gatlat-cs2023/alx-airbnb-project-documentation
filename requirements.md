# Airbnb Clone - Backend Requirements

## 1. User Authentication

### Functional Requirements
- Users can register with email/password or OAuth (Google)
- JWT token generation for authenticated sessions
- Password reset via email

### Technical Specifications
| Component            | Details                                                                 |
|----------------------|-------------------------------------------------------------------------|
| **API Endpoint**     | `POST /api/auth/register`                                               |
| **Input**           | `{ "email": "user@example.com", "password": "SecureP@ss123" }`          |
| **Validation**      | - Email format validation<br>- Password: min 8 chars, 1 special char    |
| **Output**         | `{ "token": "jwt.xyz", "user_id": "uuid" }`                            |
| **Performance**    | Response time < 500ms under 100 concurrent requests                     |

---

## 2. Property Management

### Functional Requirements
- Hosts can create/update/delete listings
- Image upload (max 5MB per image)
- Location-based search

### Technical Specifications
| Component            | Details                                                                 |
|----------------------|-------------------------------------------------------------------------|
| **API Endpoint**     | `POST /api/properties`                                                  |
| **Input**           | ```json
{
  "title": "Beach Villa",
  "description": "Luxury beachfront property",
  "price_per_night": 150,
  "location": { "lat": 34.05, "lng": -118.24 }
}
``` |
| **Validation**      | - Price must be > 0<br>- Location coordinates required                 |
| **Output**         | `{ "property_id": "uuid", "status": "published" }`                     |
| **Performance**    | Handle 50+ images/day with AWS S3 integration                          |

---

## 3. Booking System

### Functional Requirements
- Date availability checking
- Booking confirmation with payment
- Cancellation policy enforcement

### Technical Specifications
| Component            | Details                                                                 |
|----------------------|-------------------------------------------------------------------------|
| **API Endpoint**     | `POST /api/bookings`                                                    |
| **Input**           | ```json
{
  "property_id": "uuid",
  "start_date": "2025-12-01",
  "end_date": "2025-12-07"
}
``` |
| **Validation**      | - Dates must be future dates<br>- Minimum 1-night stay                  |
| **Output**         | ```json
{
  "booking_id": "uuid",
  "total_price": 1050,
  "payment_status": "pending"
}
``` |
| **Performance**    | Process 100+ concurrent bookings without date conflicts                 |

---

## Validation Rules Summary
1. **Email**: Regex `^[^\s@]+@[^\s@]+\.[^\s@]+$`
2. **Password**: Minimum 8 chars with 1 uppercase, 1 number, 1 special char
3. **Dates**: `end_date` must be after `start_date`

## Error Handling
| Code | Scenario                     | Response Body                          |
|------|------------------------------|----------------------------------------|
| 400  | Invalid dates                | `{ "error": "Invalid date range" }`    |
| 401  | Unauthorized property edit   | `{ "error": "Access denied" }`         |
| 429  | Rate limit exceeded          | `{ "error": "Too many requests" }`     |
