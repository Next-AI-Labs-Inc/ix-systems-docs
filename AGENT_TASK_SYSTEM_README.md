# IX Coach API

Backend API service for the IX Coach platform, providing authentication, coaching methods, conversation management, and integrations with third-party services.

## Table of Contents

- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [API Documentation](#api-documentation)
- [Available Scripts](#available-scripts)
- [Project Structure](#project-structure)
- [Error Handling](#error-handling)
- [Authentication](#authentication)
- [API Security Features](#api-security-features)
- [Integrations](#integrations)
- [Testing](#testing)
- [Deployment](#deployment)
- [License](#license)

## Getting Started

1. Clone the repository
2. Install dependencies:
   ```
   npm install
   ```
3. Copy the environment template:
   ```
   cp .env.example .env
   ```
4. Update environment variables in the `.env` file
5. Start the development server:
   ```
   npm run dev
   ```

## Environment Configuration

The API requires various environment variables to be set for proper operation. See `.env.example` for a complete list with descriptions.

Key configuration categories include:

- Server settings (PORT, NODE_ENV)
- Database connection
- Authentication (JWT secrets)
- Third-party API keys
- Email service
- Payment processing

## API Documentation

Interactive API documentation is available through Swagger:

- Development: http://localhost:3002/api-docs
- Production: https://api.ixcoach.com/api-docs (requires admin access)

Additional documentation is available in:

- `/docs` - Detailed implementation guides
- `/api-docs` - API usage guides (in parent directory)

## Available Scripts

- `npm run dev` - Start development server
- `npm run start` - Start production server
- `npm run test` - Run test suite
- `npm run lint` - Lint code
- `npm run build` - Build for production

## Project Structure

- `/controllers` - Request handlers
- `/models` - Database models
- `/routes` - API routes
- `/middlewares` - Authentication and error handling
- `/utils` - Helper functions
- `/config` - Configuration files
- `/docs` - Documentation files
- `/tests` - Test files

## Error Handling

The API uses a centralized error handling middleware. All errors are logged and appropriate responses are sent to the client with standardized formats.

### Standard Error Response Format

All API errors conform to the following structure:

```json
{
  "success": false,
  "requestId": "uuid-request-identifier",
  "error": "Human-readable error message",
  "errorType": "ERROR_TYPE_CODE",
  "details": [
    {
      "field": "fieldName",
      "message": "Specific validation error message",
      "value": "Problematic value (if applicable)"
    }
  ]
}
```

Error types include:
- `VALIDATION_ERROR` - Input validation failed
- `AUTHENTICATION_FAILURE` - Invalid credentials
- `AUTHORIZATION_FAILURE` - Insufficient permissions
- `NOT_FOUND` - Requested resource doesn't exist
- `DUPLICATE_ENTRY` - Resource already exists
- `INVALID_ID_FORMAT` - Malformed ID parameter
- `INVALID_REFERENCE` - Referenced resource doesn't exist
- `REFERENTIAL_INTEGRITY_VIOLATION` - Can't delete due to dependencies
- `RATE_LIMIT_EXCEEDED` - Too many requests
- `SECURITY_VIOLATION` - Potential security threat detected
- `SERVER_ERROR` - Internal server error

## Authentication

JWT-based authentication with token refresh mechanism. See `/docs/authentication.md` for details.

## API Security Features

The IX Coach API implements multiple layers of security to protect against common attack vectors:

### Input Validation and Sanitization
- All user inputs are validated using Joi schema validation
- MongoDB-specific sanitization to prevent NoSQL injections
- Custom validation patterns to detect suspicious input patterns
- Strict data type enforcement and schema validation

### Request Rate Limiting
- General rate limiting for all API endpoints
- Stricter rate limits for write operations
- Rate limit information in response headers
- Different limits for authenticated vs. unauthenticated requests

### Authentication and Authorization
- API key authentication for all requests
- JWT token validation with proper expiration
- Role-based access control
- User attribution for all operations
- Audit logs for security-sensitive operations

### Request Tracing
- Unique request ID assigned to every request
- Request IDs included in all responses
- Request IDs in log entries for correlation
- Complete request logging for troubleshooting

### Other Security Measures
- HTTPS-only in production
- Security headers (Helmet middleware)
- CORS protection
- MongoDB injection prevention
- Brute force protection
- Secure error messages (no information leakage)

## Initiatives API

The Initiatives API provides comprehensive management of initiative records, including creation, retrieval, updates, and deletion operations. It implements robust security features, validation, and follows RESTful design principles.

### Key Features

- **Comprehensive Validation**: All initiative data is validated against a strict schema
- **Input Sanitization**: Protection against NoSQL injection attacks
- **Duplicate Detection**: Automatic checking for duplicate initiatives
- **Robust Filtering**: Search by multiple criteria including text search
- **Pagination**: Efficient retrieval of large datasets
- **Sorting**: Flexible sorting options
- **Version History**: Track changes to initiatives over time
- **Referential Integrity**: Prevent orphaned references
- **Bulk Operations**: Support for creating multiple initiatives at once

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|--------------|
| GET | `/api/developer/initiatives` | List initiatives with filtering, pagination, and sorting |
| GET | `/api/developer/initiatives/:id` | Get single initiative by ID |
| POST | `/api/developer/initiatives/check-duplicates` | Check if an initiative already exists |
| POST | `/api/developer/initiatives` | Create a new initiative |
| POST | `/api/developer/initiatives/bulk` | Bulk create multiple initiatives |
| PUT | `/api/developer/initiatives/:id` | Update an existing initiative |
| DELETE | `/api/developer/initiatives/:id` | Delete an initiative |
| POST | `/api/developer/initiatives/cleanup-duplicates` | Cleanup duplicate initiative records |

### Using the API

#### Authentication

All requests require an API key in the `X-API-Key` header:

```
X-API-Key: your-api-key-here
```

For requests that require user attribution, additional headers can be included:

```
X-User-Name: username
X-User-ID: user-id
X-User-Role: user-role
```

#### Creating an Initiative

```http
POST /api/developer/initiatives
Content-Type: application/json
X-API-Key: your-api-key-here

{
  "name": "Initiative Name",
  "description": "Detailed description of the initiative",
  "status": "planned",
  "priority": "high",
  "startDate": "2025-04-01",
  "targetDate": "2025-04-30",
  "owner": "Team Lead",
  "project": "Project Name",
  "tags": ["feature", "important"],
  "keyRisks": ["Resource availability", "Technical complexity"],
  "dependencies": ["65f3a2e5c823e1b7e9a1c3d7"]
}
```

#### Checking for Duplicates

```http
POST /api/developer/initiatives/check-duplicates
Content-Type: application/json
X-API-Key: your-api-key-here

{
  "name": "Initiative Name",
  "project": "Project Name"
}
```

#### Retrieving Initiatives with Filtering

```http
GET /api/developer/initiatives?project=ProjectName&status=in-progress,planned&priority=high&limit=10&offset=0&sortBy=targetDate&sortDirection=asc
X-API-Key: your-api-key-here
```

### Error Handling

The Initiatives API follows the standardized error response format described in the Error Handling section.

## Integrations

- **HubSpot** - Customer relationship management
- **Stripe** - Payment processing
- **SendGrid** - Email delivery
- **Pusher** - Real-time updates
- **OpenAI** - AI-powered coaching

### Stripe Integration Components

The API includes robust utilities for handling Stripe webhook events and safely extracting user information:

#### Stripe Webhook Testing

We've implemented a standardized approach for Stripe webhook testing:

- **CONFIRMED Mock Data System** - All webhook tests must use verified Stripe data structures
- **Mock Data Templates** - Available in `/subscriptions-and-stripe-webhooks/mocks/`
- **Verification Tools** - Run `npm run stripe:verify-mocks` to ensure compliance

#### Stripe Integration Documentation

Comprehensive documentation is available for Stripe integrations:

- [Stripe Webhook Testing Guide](/docs/stripe-webhook-testing-guide.md) - Best practices for testing webhook handlers
- [Stripe Trial Handling Guide](/docs/stripe-trial-handling-guide.md) - How trial periods are detected and managed

#### Development Guidelines

When working with Stripe integrations:

1. **NEVER assume Stripe data structures** - Always verify with confirmed mock data
2. Use the Stripe CLI to generate confirmed webhook payloads: `stripe trigger event_name`
3. Store mock data with the `CONFIRMED_` prefix in the mocks directory
4. Validate all webhook handlers with `npm run stripe:verify-mocks`
5. Add comprehensive error handling for Stripe API failures

## Testing

Run all tests with:

```
npm run test
```

For specific test files or categories:

```
npm run test -- controllers/authController.test.js
npm run test:stripe          # Test Stripe-related functionality
npm run test:webhook         # Test webhook handlers
npm run test:trial-detection # Test trial period detection
npm run test:warnings        # Test warning log handlers
```

### Warning Log Tests

The system includes tests that verify proper handling of various edge cases that produce warnings in production logs. To run these tests specifically:

```
npm run test:warnings
```

These tests help ensure that defensive programming patterns continue to work correctly and that the system handles problematic inputs gracefully. For more information, see [Warning Log Tests Documentation](./docs/WARNING_LOG_TESTS.md).

### Stripe Testing

For Stripe-specific testing:

```bash
# Verify all Stripe webhook tests use CONFIRMED mock data
npm run stripe:verify-mocks

# Test trial period detection in webhook handlers
npm run test:trial-detection

# Run all Stripe and subscription-related tests
npm run test:subscriptions
```

## Deployment

The API is deployed on Vercel. See `vercel.json` for configuration.

## License

This project is licensed under the [Next AI Labs Inc Proprietary License](../LICENSE.md). All rights reserved. This codebase is proprietary and confidential, and usage is strictly governed by the terms outlined in the license agreement.

© 2025 Next AI Labs Inc. All rights reserved.