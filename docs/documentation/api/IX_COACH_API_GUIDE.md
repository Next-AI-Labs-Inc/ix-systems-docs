# IX Coach API - Developer Guide

## Important Features

### API Documentation (Swagger)
Interactive API documentation is available at `/api-docs` when the server is running. We've created a centralized access point at:

```
/api-documentation.html  (in the parent directory above ixcoach-api)
```

This HTML page will direct you to the Swagger documentation with proper authentication.

Alternatively, you can access Swagger directly:

```bash
# Start the server from the ixcoach-api directory
cd ixcoach-api
npm start
```

Then visit: http://localhost:3002/api-docs

**Environment-Specific Access:**
- Development: Requires admin role (can be bypassed with SWAGGER_SKIP_AUTH=true env var)
- Staging: Requires admin role
- Production: Disabled by default (can be enabled with ENABLE_SWAGGER=true env var)

If you're not authenticated, you'll be redirected to the login page and then back to Swagger after login.

### Change Review System
A system for reviewing AI-generated code changes with:
- API endpoints for creating and reviewing change requests
- Utilities for diff generation
- Client for interacting with the platform

**Key Files:**
- `/ixcoach-api/CHANGE_REVIEW_SYSTEM.md` - Main entry point documentation
- `/ixcoach-api/docs/change-review/` - Detailed documentation
- `/ixcoach-api/controllers/admin/changeRequestController.js` - API controller
- `/ixcoach-api/utils/changeRequestUtils.js` - Utilities
- `/ixcoach-api/utils/ai/changeReviewClient.js` - Client

### Documentation for Developers

**API Documentation Guide:**
- `/ixcoach-api/docs/API_DOCUMENTATION_GUIDE.md` - How to document API endpoints with Swagger

**Change Review Documentation:**
- `/ixcoach-api/docs/change-review/01-PLATFORM_OVERVIEW.md` - System architecture
- `/ixcoach-api/docs/change-review/02-IMPLEMENTATION_DETAILS.md` - Implementation details
- `/ixcoach-api/docs/change-review/03-API_INTEGRATION_GUIDE.md` - Integration guide
- `/ixcoach-api/docs/change-review/04-CHANGE_REQUEST_GUIDE.md` - Creating change requests
- `/ixcoach-api/docs/change-review/05-ISSUE_DETECTION_GUIDE.md` - Detecting issues

## Quick Start Commands

```bash
# Start the API server
cd ixcoach-api
npm start

# Run tests
cd ixcoach-api
npm test

# View API documentation
# 1. Start server
# 2. Visit http://localhost:3002/api-docs
```

## Repository Structure

- `/ixcoach-api` - Backend API
  - `/controllers` - Request handlers
  - `/routes` - API routes
  - `/models` - Database models
  - `/utils` - Utility functions
  - `/docs` - Documentation
  - `/middleware` - Express middleware
  - `/tests` - Test files

## Security Fixes & Updates

Recent security improvements:
- Password synchronization between Firebase and MongoDB
- JWT token expiration consistency
- Authentication middleware improvements
- Race condition fixes in user registration
- Standardized error handling

## Deployment

The API is deployed in several environments:
- Development: http://localhost:3002
- Staging: [staging URL]
- Production: [production URL]

## Getting Help

- Check documentation in `/ixcoach-api/docs/`
- Refer to Swagger documentation at `/api-docs` 
- For API issues, check error logs in the console