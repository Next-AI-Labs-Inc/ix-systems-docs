# Developer Task Management System Demo

## Setup and Running the System

1. **Start the API Server**

   ```bash
   cd /Users/jedi/react_projects/ix/ixcoach-api
   npm run dev
   ```

2. **Create a Demo Task**

   ```bash
   cd /Users/jedi/react_projects/ix/ixcoach-api
   node scripts/create-demo-task.js
   ```

   This will create a task in the MongoDB database and verify that it was created successfully.

3. **Run the Agent Example**

   ```bash
   cd /Users/jedi/react_projects/ix/ixcoach-api
   node examples/agent-task-creator.js
   ```

   This demonstrates how an AI agent can create tasks from any directory using the client library.

## API Endpoints

Test the API endpoints using cURL:

### Get All Tasks

```bash
curl -X GET \
  http://localhost:3002/api/developer/tasks \
  -H 'X-API-Key: dev-api-key'
```

### Create a Task

```bash
curl -X POST \
  http://localhost:3002/api/developer/tasks \
  -H 'Content-Type: application/json' \
  -H 'X-API-Key: dev-api-key' \
  -d '{
    "title": "API Test Task",
    "description": "Task created via API",
    "priority": "medium",
    "project": "ixcoach-api"
  }'
```

### Update a Task

```bash
curl -X PUT \
  http://localhost:3002/api/developer/tasks/TASK_ID_HERE \
  -H 'Content-Type: application/json' \
  -H 'X-API-Key: dev-api-key' \
  -d '{
    "status": "in-progress"
  }'
```

## Next Steps

1. **UI Integration**
   - Modify the Task UI to consume tasks from the MongoDB API

2. **Backward Compatibility**
   - Create import/export scripts for .ix-sync/tasks.json files

3. **WebSocket Support**
   - Add real-time updates through WebSockets

4. **Authentication Enhancement**
   - Improve the authentication mechanism beyond simple API keys