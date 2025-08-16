# IX Change Review System

## Overview

The IX Change Review System is a specialized platform designed to streamline the review process for AI-generated code changes. This document provides guidance for Claude on how to use and interact with this system.

## Account Management Tool Improvements - April 9, 2025

### Next Steps for User Navigation Improvements

1. Fix all remaining `returnToMainMenu()` functions to return to the operations menu instead:
   - Change all instances so tasks complete in the context where they were started
   - Use the pattern: 
   ```javascript
   setTimeout(() => {
     console.log('\nPress any key to continue...');
     stdin.once('data', () => {
       showOperationsMenu(email);
     });
   }, 1000);
   ```

2. Fix the full account deletion functionality to use the menu pattern:
   - Replace the text-based "DELETE" confirmation with a menu-based approach
   - Ensure escape key always goes back exactly one step
   - Maintain context after completing operations

3. Update error handling across all functions:
   - Add appropriate try/catch blocks
   - Never exit the application due to errors
   - Always show a descriptive error message
   - Use the same "press any key to continue" pattern after errors

4. Fix the multiple customer selection in Stripe customer deletion:
   - Replace the text-based input with a menu-based approach
   - Update indentation and syntax throughout the file
   - Ensure proper nesting of async functions

5. Make sure "Back" actions are consistent:
   - All "back" options should behave the same way
   - Users should never be unexpectedly taken to the main menu
   - Operations menu should maintain email context between tasks

All changes should follow the same pattern of maintaining context, providing simple confirmation menus with arrow key navigation, and ensuring users never need to type specific text to confirm actions.

## Documentation Location

Detailed documentation for the Change Review System is located in:

```
/docs/change-review/
```

This directory contains the following documents:

1. **README.md** - Overview and table of contents
2. **01-PLATFORM_OVERVIEW.md** - System architecture and components
3. **02-IMPLEMENTATION_DETAILS.md** - Implementation details and API endpoints
4. **03-API_INTEGRATION_GUIDE.md** - How to consume the Change Review API
5. **04-CHANGE_REQUEST_GUIDE.md** - Guide for creating effective change requests
6. **05-ISSUE_DETECTION_GUIDE.md** - Guidelines for identifying and addressing issues

## Key Components

1. **Change Request API**
   - Located at `/api/admin/changes`
   - Requires admin authentication
   - Supports creating, updating, and managing change requests

2. **Client Utility**
   - Located at `utils/ai/changeReviewClient.js`
   - Provides methods for interacting with the Change Review API
   - Handles authentication, request formatting, and error handling

3. **Diff Generation Utility**
   - Located at `utils/changeRequestUtils.js`
   - Helps generate unified diffs between file versions
   - Provides validation for change request objects

## Quick Start Guide

When you identify an issue or enhancement opportunity in the codebase:

1. Analyze the issue thoroughly to understand its impact and scope
2. Implement a fix in a new branch
3. Use the client utility to create a change request:

```javascript
const ChangeReviewClient = require('./utils/ai/changeReviewClient');
const client = new ChangeReviewClient({ token: adminToken });

// Generate diff between old and new versions
const diff = client.generateFileDiff(
  '/path/to/file.js',
  oldContent,
  newContent
);

// Create a change request
const changeRequest = {
  title: "[Fix]: Descriptive title",
  description: "Detailed explanation of the issue and solution",
  priority: "HIGH",
  repository: "ixcoach-api",
  branch: "fix-branch-name",
  files: [{ path: "/path/to/file.js", diff }],
  verificationSteps: [
    {
      title: "Step 1: Verification title",
      description: "Detailed verification instructions",
      expectedOutput: "Expected result"
    }
  ]
};

// Submit the change request
await client.createChangeRequest(changeRequest);
```

## Best Practices

1. **Be Specific**: Focus on one issue per change request
2. **Provide Context**: Explain why the change is necessary
3. **Include Verification Steps**: Make it easy for reviewers to test
4. **Follow Conventions**: Use standard formats for titles and descriptions
5. **Set Appropriate Priority**: Be judicious with priority levels

For more detailed information, please refer to the documentation in the `/docs/change-review/` directory.