# IX Coach API Documentation

This document provides instructions on how to use the Swagger API documentation for the IX Coach API.

## Accessing the Documentation

The Swagger documentation is available at the following URL when running the API server:

```
http://localhost:3000/api-docs
```

For staging and production environments, the URL will be different, and the documentation is only available to admin users.

## Authentication

Many API endpoints require authentication. To authenticate:

1. Obtain a JWT token by using the login or register endpoints.
2. Click the "Authorize" button at the top of the Swagger UI.
3. Enter your JWT token in the format: `Bearer your-token-here`
4. Click "Authorize" to apply the token to all secured endpoints.

## API Categories

The API is organized into the following categories:

- **Authentication**: User login, registration, and profile management
- **Settings**: User settings management
- **Projects**: Project creation and management
- **Conversations**: Conversation management and messaging
- **Notifications**: User notifications
- **Referrals**: Referral program management
- **Custom AI**: Custom AI features
- **AB Testing**: A/B testing functionality
- **Subscription**: Subscription and payment management
- **Feedback**: User feedback submission
- **User Sessions**: User session management
- **Engagement**: User engagement and reminders
- **Error Logging**: Error logging and reporting
- **Analytics**: User analytics and tracking
- **Slack**: Slack integration
- **Bug Reports**: Bug report submission and management
- **Sync**: Data synchronization between client and server
- **User Language**: User language detection and preferences

## Making API Requests

1. Navigate to the desired endpoint in the Swagger UI.
2. Click the endpoint to expand it.
3. Click the "Try it out" button.
4. Fill in the required parameters and request body.
5. Click "Execute" to make the request.
6. The response will be displayed below the request.

## Common Response Formats

Most API endpoints return responses in the following format:

```json
{
  "success": true,
  "data": {
    // Response data here
  }
}
```

For error responses:

```json
{
  "success": false,
  "message": "Error message"
}
```

## Adding New API Documentation

When adding new API endpoints, follow these steps to document them:

1. Create Swagger schema definitions in the `swagger-components` directory.
2. Add Swagger JSDoc comments to your route handlers.
3. Ensure your route is properly registered in the Express app.
4. Restart the server to see your new documentation.

## Example JSDoc Comment

```javascript
/**
 * @swagger
 * /api/example:
 *   get:
 *     summary: Example endpoint
 *     description: Detailed description of what this endpoint does
 *     tags: [Example]
 *     security:
 *       - bearerAuth: []
 *     parameters:
 *       - in: query
 *         name: param
 *         schema:
 *           type: string
 *         description: Description of the parameter
 *     responses:
 *       200:
 *         description: Success response
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 success:
 *                   type: boolean
 *                 data:
 *                   type: object
 *       401:
 *         description: Unauthorized
 *       500:
 *         description: Server error
 */
```

## Tips for Using the Documentation

- Use the "Schemas" section to understand the structure of request and response objects.
- Check the required fields for each endpoint.
- Pay attention to the authentication requirements for each endpoint.
- Use the "Try it out" feature to test endpoints directly from the documentation.
- The search bar at the top can help you find specific endpoints.

## Sample Clients

Here are some examples of how to call these APIs from different client technologies:

### JavaScript/Fetch

```javascript
async function fetchData(endpoint, token) {
  const response = await fetch(`https://api.ixcoach.com${endpoint}`, {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    }
  });
  return response.json();
}

// Example usage
const data = await fetchData('/api/settings/userPreferences', 'your-token-here');
```

### React Query

```javascript
import { useQuery } from 'react-query';

function useSettings(settingsName) {
  return useQuery(['settings', settingsName], async () => {
    const response = await fetch(`/api/settings/${settingsName}`, {
      headers: {
        'Authorization': `Bearer ${localStorage.getItem('token')}`,
      }
    });
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  });
}

// Example usage
const { data, isLoading, error } = useSettings('userPreferences');
```

### Axios

```javascript
import axios from 'axios';

// Create an instance with the base URL
const api = axios.create({
  baseURL: 'https://api.ixcoach.com',
});

// Add a request interceptor for authentication
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Example usage
async function getUserSettings(settingsName) {
  try {
    const response = await api.get(`/api/settings/${settingsName}`);
    return response.data;
  } catch (error) {
    console.error('Error fetching settings:', error);
    throw error;
  }
}
```