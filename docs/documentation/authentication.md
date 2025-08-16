# Authentication

This document explains the authentication system used in the IX Coach API.

## Overview

The IX Coach API uses JSON Web Tokens (JWT) for authentication. All protected endpoints require a valid JWT token in the Authorization header.

## Authentication Flow

1. Client sends credentials to the `/api/auth/login` endpoint
2. Server validates credentials and returns a JWT token
3. Client includes the token in the Authorization header for subsequent requests
4. Server validates the token and processes the request if the token is valid

## Endpoints

### Login

```
POST /api/auth/login
```

Request body:

```json
{
  "email": "user@example.com",
  "password": "secure-password"
}
```

Response:

```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "123",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "user"
  }
}
```

### Register

```
POST /api/auth/register
```

Request body:

```json
{
  "email": "newuser@example.com",
  "password": "secure-password",
  "name": "New User"
}
```

Response:

```json
{
  "success": true,
  "message": "User registered successfully",
  "user": {
    "id": "124",
    "email": "newuser@example.com",
    "name": "New User",
    "role": "user"
  }
}
```

### Refresh Token

```
POST /api/auth/refresh
```

Request header:

```
Authorization: Bearer <refresh-token>
```

Response:

```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

## Token Format

The JWT token contains the following claims:

- `sub`: User ID
- `email`: User email
- `role`: User role
- `iat`: Token creation timestamp
- `exp`: Token expiration timestamp

## Using Authentication

Include the JWT token in the Authorization header for all requests to protected endpoints:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Error Handling

Authentication errors return appropriate HTTP status codes:

- 401: Unauthorized - Invalid or expired token
- 403: Forbidden - Valid token but insufficient permissions
- 400: Bad Request - Invalid request format