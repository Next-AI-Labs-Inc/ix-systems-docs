# IX Coach API - Security Fixes Implementation Guides

This document provides comprehensive guides for implementing each security fix. Each guide includes the issue description, implementation steps, testing procedures, and verification methods.

## Table of Contents

1. [Password Synchronization Fix](#1-password-synchronization-fix)
2. [JWT Expiration Standardization](#2-jwt-expiration-standardization)
3. [Authentication Backdoor Removal](#3-authentication-backdoor-removal)
4. [User Registration Race Condition Fix](#4-user-registration-race-condition-fix)
5. [Secure Error Handling Implementation](#5-secure-error-handling-implementation)

---

## 1. Password Synchronization Fix

### Issue
Passwords updated in Firebase were not being updated in MongoDB, causing login failures. Users who reset their passwords were unable to log in because the login function authenticates against MongoDB, not Firebase.

### Branch
`hotfix-password-sync`

### Files Modified
- `controllers/authController.js`

### Implementation Steps
1. In `controllers/authController.js`, locate the `updatePassword` function (around line 627)
2. After the Firebase password update, add code to update the MongoDB User model:
   ```javascript
   // Update the user's password in Firebase
   await admin.auth().updateUser(user.uid, {
     password: password,
   });

   // ADDED: Update the password in MongoDB as well
   userData.password = password;
   await userData.save();
   ```

### Testing Steps
1. Find a test user or create a new one
2. Reset/update the user's password using the app's password update function
3. Try to log in with the new password
4. Verify the login is successful

### Verification
The fix is working when:
- Users can successfully log in after resetting or updating their password
- No "Invalid credentials" errors occur when using a recently updated password

---

## 2. JWT Expiration Standardization

### Issue
There was an inconsistency in JWT token expiration times:
- Login function issued tokens with 2-minute expiration (`expiresIn: '2m'`)
- Registration function issued tokens with 48-hour expiration (`expiresIn: '48h'`)

This caused users to be unexpectedly logged out after only 2 minutes when using the normal login flow.

### Branch
`hotfix-jwt-expiration-mismatch`

### Files Modified
- `controllers/authController.js`
- Added: `__tests__/jwt-expiration.test.js`
- Added: `__tests__/run-jwt-test.js`
- Added: `utils/testFramework.js`
- Added: `docs/hotfix-jwt-expiration-mismatch.md`

### Implementation Steps
1. In `controllers/authController.js`, locate the `login` function (around line 557)
2. Update the token expiration from '2m' to '48h':
   ```javascript
   const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, {
     expiresIn: '48h', //make sure to leave this with the same expire time as the frontend to work properly.
   });
   ```

### Testing Steps

#### Automated Testing
1. Run the JWT token expiration tests:
   ```bash
   node __tests__/run-jwt-test.js
   ```
2. Verify all tests pass, especially the test confirming login tokens now use 48h expiration

#### Manual Testing
1. Set up two test users (or use existing ones):
   - User A: A newly registered user
   - User B: An existing user who will log in normally

2. Testing the Login Flow:
   - Log in as User B through the normal login flow
   - In Chrome DevTools (F12), go to the Application tab
   - Select "Storage" → "Local Storage" → your domain
   - Find the JWT token and copy it
   - Decode the token at https://jwt.io/
   - Verify the expiration time is set to 48 hours from the issued time (not 2 minutes)
   - Wait 5 minutes and verify the user is still logged in (previously would have been logged out)

3. Testing the Registration Flow:
   - Register as User A
   - Verify a token is issued and decode it as above
   - Confirm it also has a 48-hour expiration

### Verification
The fix is successfully implemented when:
1. All automated tests pass
2. When decoded, JWT tokens from both login and registration have the same 48-hour expiration time
3. Users remain logged in for longer than 2 minutes after using the login flow
4. No errors occur in the console related to authentication

---

## 3. Authentication Backdoor Removal

### Issue
The authentication middleware contained a backdoor that allowed bypassing authentication when the request origin included 'ixsstaging'. This created a serious security vulnerability where attackers could potentially access protected endpoints by spoofing the origin header.

### Branch
`hotfix-auth-backdoor`

### Files Modified
- `middlewares/auth.js`
- Added: `__tests__/auth-backdoor.test.js`
- Added: `__tests__/run-auth-backdoor-test.js`
- Added: `utils/testFramework.js`
- Added: `docs/hotfix-auth-backdoor.md`

### Implementation Steps
1. In `middlewares/auth.js`, locate and remove the conditional backdoor code:
   ```javascript
   // REMOVE THIS CODE:
   const freeAccessToDisasterResponseAI =
     req.headers.origin?.includes('ixsstaging') || false;

   if (freeAccessToDisasterResponseAI && !authHeader) {
     req.user = await getFreeGlobalUser();
     req.body.supporterModeOrIncognitoMode = { title: 'Incognito Mode' };
     req.freeAccessToDisasterResponseAI = true;
     return next();
   }
   ```

2. Replace it with secure code that logs attempts to exploit the old vulnerability:
   ```javascript
   // Log any attempts to access the API using the old backdoor pattern
   if (req.headers?.origin?.includes('ixsstaging') && !authHeader) {
     logger.warn(`Security: Blocked unauthenticated access attempt from origin: ${req.headers.origin}`);
   }
   ```

### Testing Steps

#### Automated Testing
1. Run the authentication backdoor tests:
   ```bash
   node __tests__/run-auth-backdoor-test.js
   ```
2. Verify all tests pass, especially the test confirming the backdoor has been removed

#### Manual Testing
1. Make a request to a protected endpoint with no authentication:
   ```bash
   curl -H "Origin: https://ixsstaging.example.com" https://your-api-url/api/protected-endpoint
   ```
2. Verify you receive a 401 Unauthorized response

### Verification
The fix is successfully implemented when:
1. All automated tests pass
2. Requests with 'ixsstaging' in the origin header but no authentication are rejected with 401 status
3. The system logs attempted exploits of the old backdoor
4. All legitimate authenticated requests continue to work normally

---

## 4. User Registration Race Condition Fix

### Issue
A race condition in the user registration flow could lead to duplicate user records or database errors when multiple concurrent requests attempted to register the same user.

### Branch
`hotfix-registration-race-condition`

### Files Modified
- `controllers/authController.js`
- Added: `__tests__/race-condition.test.js`
- Added: `__tests__/run-race-condition-test.js`
- Added: `utils/testFramework.js`
- Added: `docs/hotfix-race-condition-registration.md`

### Implementation Steps
1. In `controllers/authController.js`, replace the vulnerable find-then-create pattern with atomic MongoDB operations:
   ```javascript
   // Replace:
   let user = await User.findOne({
     $or: [
       { email: caseInsensitiveEmailSearch(email) },
       { email: phoneNumber },
     ],
   });
   // ...
   if (!user) {
     // Create user
     // ...
   }

   // With:
   // Create a new user with atomic operation to prevent race conditions
   const result = await User.findOneAndUpdate(
     { email: identifier.toLowerCase() },
     {
       $setOnInsert: {
         email: identifier.toLowerCase(),
         // user properties
       }
     },
     { 
       upsert: true, 
       new: true,
       setDefaultsOnInsert: true
     }
   );
   
   user = result;
   ```

### Testing Steps

#### Automated Testing
1. Run the race condition tests:
   ```bash
   node __tests__/run-race-condition-test.js
   ```
2. Verify all tests pass, especially the test confirming the fix prevents race conditions

#### Manual Testing
1. Use a tool like Apache Bench to create concurrent registration requests:
   ```bash
   ab -n 10 -c 5 -p registration_payload.json -T application/json https://your-api-url/api/firebase-register
   ```
2. Check the database for duplicate records:
   ```bash
   # Using MongoDB CLI
   db.users.find({email: "test@example.com"}).count()
   ```
3. Verify only one user record exists

### Verification
The fix is successfully implemented when:
1. All automated tests pass
2. Concurrent registration attempts with the same email result in only one user record
3. No database errors occur during concurrent registrations
4. The registration flow works correctly for all users

---

## 5. Secure Error Handling Implementation

### Issue
Error handlers in the codebase were directly exposing raw error messages to clients, potentially leaking sensitive information like database connection strings, stack traces, or system details.

### Branch
`hotfix-error-handling-security`

### Files Modified
- `controllers/authController.js`
- Added: `utils/secureErrorHandler.js`
- Added: `__tests__/secure-error-handler.test.js`
- Added: `__tests__/run-secure-error-handler-test.js`
- Added: `utils/testFramework.js`
- Added: `docs/hotfix-secure-error-handling.md`

### Implementation Steps
1. Create a new utility for secure error handling: `utils/secureErrorHandler.js`
2. Update error handling in authentication controller functions:
   - `register`
   - `login`
   - `startTrial`
   - `getProfile`
   - `updatePassword`
   - `getSubscriptionDetails`

3. For each function, replace:
   ```javascript
   } catch (err) {
     console.log(err);
     res.status(400).json({ error: err.message });
   }
   ```

   With:
   ```javascript
   } catch (err) {
     // Log detailed error for debugging
     logger.error(`Error description: ${err.message}`, {
       userId: req.user.userId,
       stack: err.stack
     });
     
     // Return sanitized error to client
     return res.status(400).json({
       error: {
         type: 'ERROR_TYPE',
         message: 'Generic error message that doesn\'t expose sensitive details'
       }
     });
   }
   ```

### Testing Steps

#### Automated Testing
1. Run the secure error handler tests:
   ```bash
   node __tests__/run-secure-error-handler-test.js
   ```
2. Verify all tests pass, especially tests confirming sanitized error responses

#### Manual Testing
1. Trigger various errors in different endpoints:
   - Submit invalid data to authentication endpoints
   - Attempt to access resources that don't exist
   - Force validation errors
   
2. For each error:
   - Check the response doesn't contain raw error messages or stack traces
   - Verify detailed errors are logged server-side
   - Confirm the error response contains a generic message and error type

### Verification
The fix is successfully implemented when:
1. All automated tests pass
2. Error responses don't contain sensitive information
3. Server logs contain detailed error information for debugging
4. Error responses follow a standardized format
5. The system maintains appropriate security boundaries between client and server

---

## 6. Stripe Payment System Logging Refactoring

### Issue
The Stripe payment controller (`stripeController.js`) was a large, complex file (2,472 lines) with significant code duplication in its logging functions. This made it difficult to maintain and modify, increasing the risk of bugs when updating payment processing code.

### Branch
`refactor-stripe-logger`

### Files Modified
- Modified: `controllers/subscription/stripeController.js`
- Added: `utils/stripeLogger.js`
- Added: `__tests__/utils/stripeLogger.test.js`
- Added: `__tests__/utils/stripe-ux-impact-tests.js`
- Added: `__tests__/utils/stripe-extraction-verification.test.js`
- Added: `test-utils/user-impact-reporter.js`
- Added: `run-stripe-tests-clean.sh`
- Added: `jest.config.js`

### Implementation Steps
1. Identify repeated logging patterns in stripeController.js
2. Extract each pattern into specialized functions in a new utility file (stripeLogger.js)
3. Create tests that verify the utility functions work correctly
4. Update the stripeController.js to use the new utility functions
5. Create user-focused tests that verify the refactoring doesn't break critical payment flows

### Testing Approach
For this refactoring, we implemented a comprehensive test-driven approach:

#### Unit Tests
- Test individual logger functions with various inputs
- Verify proper formatting and content of log messages
- Mock dependencies to isolate logging functionality

#### User Experience Tests
- Test real user payment journeys (signup, checkout, renewal, etc.)
- Include `(USER IMPACT: ...)` and `(BUSINESS IMPACT: ...)` annotations
- Verify critical payment flows aren't affected by the refactoring

#### Custom Testing Tools
- Added a custom Jest reporter for clearer test output
- Created a test runner script for human-readable results
- Configured Jest to support these custom tools

### Testing Steps

#### Automated Testing
1. Run the unit tests for the logger utility:
   ```bash
   npm test -- __tests__/utils/stripeLogger.test.js
   ```

2. Run the user experience tests:
   ```bash
   ./run-stripe-tests-clean.sh
   ```

3. Verify extraction was done correctly:
   ```bash
   npm test -- __tests__/utils/stripe-extraction-verification.test.js
   ```

### Verification
The refactoring is successful when:
1. All tests pass
2. The controller file is shorter and more maintainable
3. All logging functionality works identically to before
4. User payment flows are unaffected

## Creating Pull Requests

For each fix above, create a pull request using the following URL pattern:
```
https://github.com/jryanhaber/ixcoach-api/pull/new/[branch-name]
```

For example:
- https://github.com/jryanhaber/ixcoach-api/pull/new/hotfix-jwt-expiration-mismatch
- https://github.com/jryanhaber/ixcoach-api/pull/new/hotfix-auth-backdoor
- https://github.com/jryanhaber/ixcoach-api/pull/new/refactor-stripe-logger

Use the information from this guide to fill in the pull request description, ensuring you include:
1. Issue description
2. Implementation details
3. Testing steps
4. Verification methods