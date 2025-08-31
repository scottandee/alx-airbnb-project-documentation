# Backend Requirement Specifications  
This document defines the detailed requirements for the **Airbnb Clone (Booking Management System)** backend.  
It covers **API endpoints, input/output specifications, validation rules, and performance criteria** for core features.  

---

## 1. User Authentication

### Description
Handles **user registration, login, and authentication** for both guests and hosts.  
Provides secure access to the system using **JWT-based authentication**.

### API Endpoints
- `POST /api/auth/register` – Register new user (Guest/Host).  
- `POST /api/auth/login` – Authenticate user and return JWT token.  
- `GET /api/auth/profile` – Fetch user profile (requires authentication).  
- `PUT /api/auth/profile` – Update user profile (requires authentication).  

### Input / Output Specifications
**Register (POST /api/auth/register)**  
**Request:**
```json
{
  "name": "John Doe",
  "email": "johndoe@example.com",
  "password": "SecurePass123!",
  "role": "guest"
}
```
**Response:**
```json
{
  "message": "User registered successfully",
  "userId": "u12345"
}
```
**Login (POST /api/auth/login)**

**Request:**
```json
{
  "email": "johndoe@example.com",
  "password": "SecurePass123!"
}

```
**Response:**
```json
{
  "token": "jwt-token-string",
  "expiresIn": 3600
}
```

### Validation Rules
- **Email** must be unique and valid (`name@domain.com`).  
- **Password** must be minimum 8 characters, including uppercase, number, and symbol.  
- **Role** must be either `guest` or `host`.  
- **Authentication** required for all protected routes.  

### Performance Criteria
- Registration and login responses should complete within **500ms** under normal load.  
- Authentication system must support at least **1000 concurrent login requests**.  
- **JWT tokens** must expire after a configurable duration (default: **1 hour**).  

---

## 2. Property Management

### Description
Enables **hosts** to add, update, and delete property listings.  
Supports property details like **title, description, location, price, amenities, and availability**.  

### API Endpoints
- `POST /api/properties` – Add new property (Host only).  
- `GET /api/properties` – Get all properties (supports search & filter).  
- `GET /api/properties/:id` – Get property details by ID.  
- `PUT /api/properties/:id` – Update property details (Host only).  
- `DELETE /api/properties/:id` – Delete property (Host only).  

### Input / Output Specifications

**Add Property (POST /api/properties)**  
**Request:**  
```json
{
  "title": "Modern Apartment in Lagos",
  "description": "2-bedroom apartment near the beach",
  "location": "Lagos, Nigeria",
  "price_per_night": 120,
  "amenities": ["WiFi", "Pool", "Air Conditioning"],
  "availability": {
    "startDate": "2025-09-01",
    "endDate": "2025-09-30"
  }
}
```
**Response:**
```json
{
  "message": "Property listed successfully",
  "propertyId": "p67890"
}
```
### Validation Rules
- **Title** must not exceed **100 characters**.  
- **Price** must be a **positive number**.  
- **Location** must be provided.  
- **Amenities** should be an **array of strings**.  
- **Availability** must include valid date ranges (**startDate < endDate**).  

### Performance Criteria
- **Property search** should return results within **700ms** for datasets up to **100,000 listings**.  
- API must support **pagination** for large property lists.  
- **Image upload** (if included later) should use **cloud storage** (e.g., AWS S3).  
