# API Documentation Guide

This guide explains how to document API endpoints using Swagger/OpenAPI in the IX Coach API.

## Overview

The IX Coach API uses [Swagger](https://swagger.io/) (OpenAPI) for interactive API documentation. This provides:

1. Interactive documentation accessible at `/api-docs`
2. Machine-readable API specifications
3. Testing interface to try out API calls directly from the browser
4. Clear documentation of request/response schemas

## Getting Started

To document an API endpoint, you'll add JSDoc-style comments to your route definition files.

### Basic Example

Here's a simple example of documenting a GET endpoint:

```javascript
/**
 * @swagger
 * /api/users:
 *   get:
 *     summary: Get all users
 *     tags: [Users]
 *     security:
 *       - bearerAuth: []
 *     responses:
 *       200:
 *         description: List of users
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 $ref: '#/components/schemas/User'
 *       401:
 *         description: Unauthorized
 */
router.get('/', userController.getAllUsers);
```

### Documenting POST Endpoints

For POST endpoints that accept request bodies:

```javascript
/**
 * @swagger
 * /api/users:
 *   post:
 *     summary: Create a new user
 *     tags: [Users]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required:
 *               - email
 *               - password
 *             properties:
 *               email:
 *                 type: string
 *                 format: email
 *               password:
 *                 type: string
 *                 format: password
 *     responses:
 *       201:
 *         description: User created successfully
 *       400:
 *         description: Invalid input
 */
router.post('/', userController.createUser);
```

## Schema Definitions

For complex objects, define schemas in the `swagger-components/schemas.js` file:

```javascript
/**
 * @swagger
 * components:
 *   schemas:
 *     MyModel:
 *       type: object
 *       properties:
 *         id:
 *           type: string
 *         name:
 *           type: string
 *         createdAt:
 *           type: string
 *           format: date-time
 */
```

Then reference these schemas in your route documentation:

```javascript
/**
 * @swagger
 * /api/my-model/{id}:
 *   get:
 *     summary: Get model by ID
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: Success
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/MyModel'
 */
```

## Authentication

For endpoints that require authentication, add the security section:

```javascript
/**
 * @swagger
 * /api/protected-endpoint:
 *   get:
 *     security:
 *       - bearerAuth: []
 *     // rest of the documentation
 */
```

## Best Practices

1. **Group Related Endpoints**: Use the `tags` property to group related endpoints
2. **Be Clear and Concise**: Write clear summaries and descriptions
3. **Document All Parameters**: Include all required and optional parameters
4. **Specify Response Formats**: Document the expected response format for each endpoint
5. **Include Error Responses**: Document possible error responses and their codes
6. **Use Schema References**: For complex objects, define schemas and reference them
7. **Add Examples**: Provide examples where helpful

## Examples

Check these files for examples:

- `/routes/admin/changeRequests.js` - Full endpoint documentation
- `/swagger-components/schemas.js` - Schema definitions
- `/routes/auth.js` - Authentication endpoints

## Testing Your Documentation

After adding documentation, visit `/api-docs` in your browser to see the interactive API documentation. You can test your endpoints directly from this interface.

## Further Reading

- [Swagger/OpenAPI Specification](https://swagger.io/specification/)
- [Swagger JSDoc Documentation](https://github.com/Surnet/swagger-jsdoc/blob/master/docs/GETTING-STARTED.md)
- [Swagger UI Express](https://github.com/scottie1984/swagger-ui-express)