# IX Coach API Documentation Guide

## Overview

We've updated the Swagger documentation for the IX Coach API to include all endpoints that were previously undocumented. This guide explains what was done and how to maintain the documentation moving forward.

## What Was Added

1. **Schema Definitions**: Created comprehensive schema definitions for all API request and response objects in the `swagger-components` directory:
   - Authentication: User-related schemas
   - Settings: User settings schemas
   - Projects: Project management schemas
   - Conversations: Conversation and messaging schemas
   - Notifications: Notification schemas
   - Referrals: Referral program schemas
   - Custom AI: AI-related schemas
   - AB Testing: A/B testing schemas
   - Subscriptions: Subscription and payment schemas
   - Feedback: User feedback schemas
   - Error Logging: Error logging schemas
   - Analytics: User analytics schemas

2. **Route Documentation**: Added Swagger JSDoc comments to all route handlers, documenting:
   - URL path and method
   - Summary and detailed description
   - Request parameters and body schemas
   - Response schemas and status codes
   - Authentication requirements
   - Example requests and responses

3. **New Route Files**: Created new route files for endpoints that were missing implementation:
   - `/routes/settings.js`
   - `/routes/sync.js`
   - `/routes/engagement.js`
   - `/routes/log-error.js`
   - `/routes/slack.js`
   - `/routes/track-visit.js`
   - `/routes/user-session.js`

4. **Updated Existing Routes**: Added documentation to existing route files:
   - `/routes/auth.js`
   - `/routes/abTest.js`
   - `/routes/subscription.js`
   - `/routes/conversations.js`
   - `/routes/notifications.js`
   - `/routes/feedback.js`

5. **Configuration**: Updated the Swagger configuration to include all new components and routes.

6. **Documentation**: Created comprehensive documentation on how to use and maintain the Swagger API documentation.

## How to Access the Documentation

When running the API server, the Swagger documentation is available at:

```
http://localhost:3000/api-docs
```

## How to Maintain the Documentation

### Adding New Endpoints

When adding new endpoints, follow these steps:

1. **Add Schema Definitions**: Create or update schema definitions in the appropriate file in the `swagger-components` directory.

2. **Document Route Handler**: Add JSDoc comments above your route handler with the Swagger specification. Example:

```javascript
/**
 * @swagger
 * /api/your-endpoint:
 *   post:
 *     summary: Short summary
 *     description: Detailed description
 *     tags: [Your Tag]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             $ref: '#/components/schemas/YourRequestSchema'
 *     responses:
 *       200:
 *         description: Success response
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/YourResponseSchema'
 *       400:
 *         description: Bad request
 *       401:
 *         description: Unauthorized
 *       500:
 *         description: Server error
 */
router.post('/your-endpoint', auth, yourController.yourHandler);
```

3. **Add Examples**: Include example requests and responses where helpful:

```javascript
 *     examples:
 *       Example Name:
 *         summary: Short summary of example
 *         value:
 *           property1: "value1"
 *           property2: "value2"
```

4. **Register the Route**: Ensure your route is properly registered in the Express app.

5. **Restart the Server**: Restart the API server to see your new documentation.

### Updating Existing Documentation

To update existing documentation:

1. Locate the route handler in the appropriate route file.
2. Update the JSDoc comment with the new information.
3. Restart the server to see the changes.

## Best Practices

1. **Be Descriptive**: Provide clear, concise descriptions for endpoints and parameters.
2. **Include Examples**: Add examples for complex request bodies.
3. **Document All Responses**: Document all possible response statuses and bodies.
4. **Keep Schemas Updated**: When changing models, update the corresponding schema definitions.
5. **Organize by Tags**: Use appropriate tags to organize endpoints by feature.
6. **Maintain Security Requirements**: Always specify security requirements for protected endpoints.

## Testing Documentation

Always test your documentation changes by:

1. Starting the API server
2. Navigating to the Swagger UI (`/api-docs`)
3. Verifying that your endpoint is properly displayed
4. Testing the endpoint using the "Try it out" feature

## Code Alignment Check Results

After examining the actual implementation code, I found and corrected the following misalignments:

1. **Text-to-Speech Endpoint**: 
   - Documentation listed it as `/api/speak-the-text` 
   - Actual implementation uses `/api/audios/text-to-speech`
   - Fixed by creating proper documentation for the correct endpoint

2. **User Language Detection**:
   - Documentation suggested there was an API endpoint for this
   - Actual implementation makes direct calls to external services (`https://api.ipify.org` and `https://ipapi.co/`)
   - Fixed by updating documentation to reflect this client-side implementation

## Remaining Work

The following items may still need attention:

1. **Comprehensive Validation**: Implement a formal validation process to ensure documentation stays in sync with code
2. **More Detailed Examples**: Add more detailed examples for complex endpoints
3. **Response Examples**: Add examples of actual responses where helpful
4. **Environment-Specific URLs**: Document environment-specific URLs for staging and production
5. **Automated Testing**: Consider adding automated tests to verify API implementation matches documentation

## Conclusion

With this comprehensive API documentation, developers can now more easily understand and use the IX Coach API. The documentation serves as both a reference and a testing tool, ensuring consistent API usage across the application.

For any questions or issues with the API documentation, please create a ticket or reach out to the development team.