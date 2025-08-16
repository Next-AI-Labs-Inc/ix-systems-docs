# Bug Reporting Integration Documentation

This document explains how the frontend bug reporter components integrate with the backend API.

## Overview

The bug reporting system consists of:

1. **Frontend Components**:
   - `BugReporterButton.js`: UI entry point for reporting bugs
   - `BugReportModal.js`: Form for capturing bug details
   - `BugReporter.js`: Main orchestration component

2. **Backend Components**:
   - `BugReport.js`: Mongoose schema model
   - `bugReportController.js`: API endpoints for CRUD operations
   - `mapBugReport.js`: Utility for mapping frontend data to backend model

## Data Flow

1. User clicks on bug report button or accesses via menu
2. Bug report modal collects:
   - Basic description (`whatYouNoticed`)
   - Advanced details (title, actual/expected behavior, steps)
   - App state data (URL, browser info, viewport size)
   - Screenshots/recordings

3. Data is submitted to the API endpoint: `POST /api/bug-reports`
4. Backend maps frontend format to backend schema
5. Bug report is stored in MongoDB
6. Reports can be viewed and managed in admin dashboard

## Field Mapping

| Frontend Field | Backend Field | Notes |
|----------------|---------------|-------|
| `whatYouNoticed` | `description` | Required field |
| `title` | `title` | Required for advanced reports |
| `actualBehavior` | `actualBehavior` | Uses `whatYouNoticed` as fallback |
| `expectedBehavior` | `expectedBehavior` | Uses "Not provided" as fallback |
| `steps` | `steps` | Array of step strings |
| `appState.location` | `url` | URL where bug occurred |
| `appState.userAgent` | `browserInfo` | Browser information |
| `appState.viewport` | `deviceInfo` | Formatted as "WIDTHxHEIGHT" |
| `screenshot` | `screenshot` | Uploaded to Cloudinary |
| `loomVideoLink` | Stored in metadata | Video recording URL |

## Implementation Details

### Frontend Bug Reporting Flow

```javascript
// Example of using the BugReporter component
import BugReporter from './components/bugReporter';

function App() {
  return (
    <div>
      {/* Other app components */}
      <BugReporter isBugHunter={user.bughunter} />
    </div>
  );
}
```

### Backend API Implementation

The API is designed to handle both direct backend format and the frontend format automatically:

```javascript
// Example of submitting a bug report from frontend
const response = await fetch('/api/bug-reports', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': token ? `Bearer ${token}` : undefined
  },
  body: JSON.stringify({
    whatYouNoticed: 'Button not working',
    title: 'UI Bug',
    actualBehavior: 'Button does nothing on click',
    expectedBehavior: 'Button should navigate to next page',
    steps: ['Go to dashboard', 'Click on Start button'],
    screenshot: 'data:image/png;base64,abc123',
    appState: {
      location: 'https://ixcoach.com/dashboard',
      userAgent: 'Chrome/91.0.4472.124',
      viewport: { width: 1920, height: 1080 }
    }
  })
});
```

## Bug Hunter Program

The system includes a bug hunter program that rewards users who report bugs:

1. Users can become "bug hunters" by submitting quality bug reports
2. Bug hunters receive one month of free premium access
3. Bug hunter status is maintained in the User model with the `bughunter` field
4. Bug hunters can be identified in bug reports through the `reportedBy.isBugHunter` field
5. To become a bug hunter, authenticated users can call `PATCH /api/bug-reports/bughunter`

## Testing

Both frontend components and backend APIs have comprehensive test coverage. See:

- `__tests__/utils/mapBugReport.test.js`: Tests for frontend-to-backend mapping
- `__tests__/controllers/bugReportController.test.js`: Unit tests with mocked dependencies
- `__tests__/bug-report.test.js`: Integration tests using in-memory MongoDB

## Swagger Documentation

API documentation is available at `/api-docs` when the server is running. It includes:

- Complete schema definitions for both frontend and backend formats
- Endpoint documentation with request/response examples
- Special documentation endpoint: `GET /api/bug-reports/frontend-mapping`

## Next Steps

1. Implement screenshot capture functionality in the frontend
2. Add admin dashboard for bug report management
3. Implement bug recreation system for testing reported issues
4. Add bug hunter leaderboard and recognition system
5. Develop automated bug detection features
6. Add environment data collection system
7. Build verification and closure workflow
8. Create API integrations with developer tools
9. Implement communication system between developers and reporters