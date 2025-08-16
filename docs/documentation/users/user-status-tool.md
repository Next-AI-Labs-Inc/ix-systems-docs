# User Status Check Tool

This tool provides comprehensive user account diagnostics, helping administrators quickly diagnose login issues and verify subscription status.

## Features

- Case-insensitive email lookup
- Firebase authentication status verification
- Subscription status check with plan details
- Detailed login issue analysis with recommended solutions
- Command-line interface for quick checks
- Admin API endpoint for integration

## Use Cases

- Troubleshooting user login issues
- Verifying subscription status
- Checking firebase authentication configuration
- Diagnosing case sensitivity issues with email addresses

## How to Use

### Command Line Tool

The command line tool is the quickest way to check a user's status:

```bash
node scripts/check-user-status.js email@example.com
```

This will provide a comprehensive report including:
- User account details
- Firebase authentication status
- Subscription information
- Login issue analysis

### API Endpoint (Admin Only)

For integration with other systems, use the admin API endpoint:

```
GET /api/admin/user-status/:email
```

This endpoint requires admin authentication and returns a JSON response with the same information as the command line tool.

## Common Issues and Solutions

### Case Sensitivity

The most common issue is case sensitivity in email addresses. Our lookup utility uses case-insensitive search, but some authentication systems may be case-sensitive.

### Firebase Authentication Mismatch

Another common issue is when a user exists in MongoDB but not in Firebase authentication. The tool will detect this and recommend creating a Firebase user or resetting the password.

### Subscription Status

If a user reports having a paid account but can't access premium features, the tool will verify their subscription status and show detailed plan information.

## Implementation Details

The core of the tool is in the `utils/userLookupTool.js` file, which provides the `lookupUserStatus` function. This function:

1. Looks up a user by email with case-insensitive search
2. Checks Firebase authentication status
3. Verifies subscription status
4. Analyzes potential login issues

> **Important Note**: This tool requires importing the Plan model explicitly to ensure MongoDB model registration order is correct. Always include the Plan model import when working with this tool.

The code is designed to be robust, with proper error handling and informative messages.

## Example Output

```
📋 User Status Report:
====================

✅ User Found
User ID: 60f9e5b3c1f3d232f8c88b76
Email: customer@example.com
Status: beta
Current Plan: 2
Last Login: Mon Mar 18 2025 09:32:11 GMT-0700 (PDT)
Account Created: Fri Jul 23 2021 06:45:23 GMT-0700 (PDT)

🔐 Firebase Authentication:
  UID: 9aVxPw8Y5ZXIGmM2W6RZqS3F1234
  Email Verified: true
  Account Disabled: false
  Has Password Auth: true
  Creation Time: Fri Jul 23 2021 06:45:23 GMT-0700 (PDT)
  Last Sign In: Mon Mar 18 2025 09:32:11 GMT-0700 (PDT)

💳 Subscription Status:
  Active: true
  Plan ID: 2
  Plan Name: Pro
  Start Date: Mon Mar 11 2025 00:00:00 GMT-0700 (PDT)
  Next Reset Date: Thu Apr 11 2025 00:00:00 GMT-0700 (PDT)
  Stripe Customer ID: cus_KdJfS82jHs7Lkm
  Stripe Subscription ID: sub_1OP8jhJdS82K7LkmjHs

🔍 Login Issue Analysis:
  Issues:
    - User email in database uses mixed case which might cause login issues
  Possible Solutions:
    - Advise user to use exact case sensitivity: customer@example.com
  Recommended Login Method: Email/password login
```