# MongoDB-Based Initiative Management System

This document describes how to use the MongoDB-based Initiative Management System, which provides a central storage solution for initiatives across all IX projects.

## Overview

The Initiative Management System allows:

- Storing initiatives in a central MongoDB database
- Accessing initiatives from any project directory
- Creating, reading, updating, and deleting initiatives via a REST API
- Importing existing initiatives from `.ix-sync/initiatives.json` files

This implementation follows the same pattern as the Developer Task Management System, providing a consistent experience for both tasks and initiatives.

## API Endpoints

All endpoints require API key authentication using the `X-API-Key` header.

### Base URL

```
http://localhost:3002/api/developer/initiatives
```

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | / | Get all initiatives (with optional filtering) |
| GET | /:id | Get a specific initiative by ID |
| POST | / | Create a new initiative |
| PUT | /:id | Update an existing initiative |
| DELETE | /:id | Delete an initiative |
| POST | /cleanup-duplicates | Clean up duplicate initiatives (restricted to safe projects) |

### Query Parameters for GET /

- `project`: Filter by project name
- `projects`: Filter by multiple project names (comma-separated or array)
- `status`: Filter by status
- `priority`: Filter by priority
- `dedup`: Set to 'true' to deduplicate results by name+project

## Initiative Model Structure

Initiatives follow this structure:

```json
{
  "name": "Initiative Name",
  "description": "Detailed description",
  "status": "planned|in-progress|completed|on-hold",
  "priority": "high|medium|low",
  "startDate": "YYYY-MM-DD",
  "targetDate": "YYYY-MM-DD",
  "owner": "Owner Name",
  "budget": "Optional budget information",
  "project": "Project name",
  "tags": ["tag1", "tag2"],
  "keyRisks": ["risk1", "risk2"],
  "dependencies": ["dependency1", "dependency2"],
  "linkedProjects": ["project1", "project2"],
  "linkedKpis": ["kpi1", "kpi2"],
  "createdAt": "ISO date",
  "updatedAt": "ISO date",
  "completedAt": "ISO date or null",
  "createdBy": "Creator name or system"
}
```

## Using the Client Library

### Installation

No installation needed - the client is built into the ixcoach-api project.

### Creating Initiatives

```javascript
const { createInitiative } = require('../utils/agentInitiativeClient');

// Create a new initiative
const initiative = await createInitiative({
  name: "Initiative Name",
  description: "Detailed description",
  status: "planned",
  priority: "high",
  project: "project-name",
  // ... other fields
});
```

### Fetching Initiatives

```javascript
const { getInitiatives } = require('../utils/agentInitiativeClient');

// Get all initiatives
const allInitiatives = await getInitiatives();

// Get initiatives with filtering
const filteredInitiatives = await getInitiatives({
  project: "project-name",
  status: "in-progress",
  priority: "high"
});
```

## Importing from `.ix-sync/initiatives.json`

Use the import script to import existing initiatives:

```bash
cd ixcoach-api
node scripts/import-ix-sync-initiatives.js /path/to/project
```

The script will:
1. Find all `.ix-sync/initiatives.json` files in the specified directory
2. Import initiatives into MongoDB
3. Skip initiatives that already exist (based on name+project)
4. Add project name to initiatives if not already set

## Creating Demo Initiatives

Use the demo script to create sample initiatives:

```bash
cd ixcoach-api
node scripts/create-demo-initiative.js
```

This will create a sample "Enhanced Vector Memory System" initiative with sub-initiatives for testing purposes.

## Integration with IX CLI

The MongoDB-based Initiative Management System is designed to eventually integrate with the IX CLI, replacing the file-based storage with API calls. This will allow for a more robust and scalable system while maintaining backward compatibility.

## Security and Access Control

- API endpoints are protected with API key authentication
- For development, use the key `dev-api-key`
- For production, use environment variables to set the API key

## Best Practices

1. Always specify a project name when creating initiatives
2. Use consistent naming conventions across projects
3. Maintain relationships between initiatives and tasks
4. Use descriptive names and detailed descriptions
5. Keep the status field updated as work progresses