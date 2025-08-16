# Error Codes Registry

This registry maintains a mapping of unique 4-digit error codes to specific error locations in the codebase. When adding new error logging, please assign the next available code and document it here.

## Agent Guide for Error Code Registry

### Overview

This registry serves two primary purposes:

1. **Error Identification**: Each error is assigned a unique 4-digit code with the 🔍 emoji identifier in logs for quick searching
2. **Error Severity Tracking**: We track both current and target severity levels to monitor error reduction progress

### How to Use This Registry

1. **When investigating an error**:

   - If you see a log with `🔍1234`, search this registry for code `1234`
   - The registry will show you the exact file, location, and nature of the error
   - Use this information to find and fix the error source without extensive searching

2. **When fixing an error**:

   - Check if the error has a target level (not "TBD")
   - Implement fixes to reduce the error's severity to match the target level
   - Update the registry once the fix is implemented and verified

3. **When adding new error logging**:

   - Use the next available error code in sequence
   - Add error details to this registry following the established format
   - Set appropriate severity level based on impact
   - Initially set target level to "TBD" until reviewed
   - Format error message to clearly explain what happened using this pattern:
   
   ```javascript
   // Example error message format:
   if (customerResult.customers.data.length > 0 && !resolvedCustomer) {
     logger.error(
       `When we were trying to fetch subscription data from Stripe API, we tried to ` +
       `resolve the correct customer from multiple records so we could retrieve the most relevant subscription, but that required ` +
       `the multipleStripeCustomerResolver function to return a valid customer ID, which we expected to work reliably. ` +
       `Customer resolution failed in stripe-operations-registry.js:getSubscriptions:152. ` +
       `When processing API request for email ${email}, found ${customerResult.customers.data.length} ` +
       `Stripe customers but resolver returned null. Customer IDs: [${customerResult.customers.data.map(c => c.id).join(', ')}]. ` +
       `This will cause the subscription endpoint to return no data despite the user having a valid subscription in Stripe. ` +
       `Check multipleStripeCustomerResolver.js prioritization logic or verify this email's customers in Stripe dashboard.`
     );
   }
   ```
   
   This format explains:
   - What we were trying to do (the overall operation)
   - How we tried to do it (the specific actions)
   - What was required for success (dependencies or assumptions)
   - Where the error occurred (file, function, line number)
   - What data we found (vs. what was expected)
   - The user impact of this error
   - Suggested next steps to resolve the issue

4. **When improving error handling**:

   - Target reduction in error severity level where possible
   - Update the registry with both current and improved target levels
   - Note any backward compatibility requirements

# How to add to this

When requested peform this exact step. Scan the code for the specific item requested, check if its in here. If Not, add it. Capture the severity by checking the type it currently is against the menu. logger.error is an 8. go to that log and add the id, update the registry. In that log at the moment it is being logged, add to the message the id of it with an hourglass emoji, and under it at the end add the full path and function name on separate lines it originated from with a target emoji. Repeat until scope is completed, add nothing but these protocols without auth.

#### Error Severity Levels

The severity levels in this system are based on both user impact and team notification requirements:

- **Levels 1-5**: Local console only, no team notification (🔵 Blue)
- **Levels 6-8**: Team notification, moderate severity (🟠 Orange)
- **Levels 9-10**: Team notification, high severity (🔴 Red)
- **Level 11+**: Team notification, critical severity (🟣 Purple) - requires immediate action

## Logging Methods

⚠️ **IMPORTANT**: Never use any `logger.*` methods without explicit human confirmation. These notify the entire team via Slack and can cause significant disruption.

### 1. Console Methods (Local Only)

These methods only log to the local console and do not notify anyone. Safe to use without permission.

| Method        | Significance | Usage                                                                                                                                                                                                                                                           | Agent Usage Policy |
| ------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| console.log   | 1 🔵         | Outputs ONLY to local console/terminal. NOBODY else sees these logs. They do not persist in monitoring systems. They are purely for local debugging and disappear when the console is cleared. Appropriate for temporary debugging and development information. | ✅ Safe to use     |
| console.warn  | 6 🟠         | Outputs ONLY to local console/terminal with warning formatting (usually yellow). NOBODY else sees these logs. They do not persist in monitoring systems. Use for potential issues that don't break functionality but should be reviewed during development.     | ✅ Safe to use     |
| console.error | 8 🔴         | Outputs ONLY to local console/terminal with error formatting (usually red). NOBODY else sees these logs. They do not persist in monitoring systems. Use for errors that need local attention but don't require team notification.                               | ✅ Safe to use     |

### 2. Core Logger Methods (Team Notification)

These methods notify the entire team via Slack and should never be used without explicit permission.

| Method       | Significance | Usage                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Agent Usage Policy                       |
| ------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| logger.info  | 6 🟠         | POSTS TO SLACK - Notifies ENTIRE DEVELOPMENT TEAM via dedicated info channel. Every team member's Slack lights up. Previously caused 4+ hours of cleanup work when misused. Use ONLY for significant events that the entire team needs to know about immediately. Use this when a human has requested logging of a successful operation, never assume this is desired or assign yourself tasks to do this without confirming it aligns with human intent.           | ❌ NEVER use without explicit permission |
| logger.warn  | 8 🟠         | POSTS TO SLACK - Notifies ENTIRE DEVELOPMENT TEAM via dedicated warnings channel with high-visibility format. MULTIPLE team notifications occur. Every team member's Slack lights up with warning notification. Previously caused 4+ hours of cleanup work when misused. Use this when a human has requested logging of a less critical operation, never assume this is desired or assign yourself tasks to do this without confirming it aligns with human intent. | ❌ NEVER use without explicit permission |
| logger.error | 10 🔴        | POSTS TO SLACK - Notifies ENTIRE DEVELOPMENT TEAM via critical error channel with highest-visibility. Every team member's Slack lights up with critical notification. Use this After confirmation from a human every case for critical operational errors, never assume this is desired or assign yourself tasks to do this without confirming it aligns with human intent.                                                                                         | ❌ NEVER use without explicit permission |
| logger.error | 9 🔴         | POSTS TO SLACK - Same as logger.error, but for errors with slightly lower severity that still require immediate attention. Routes to same function as logger.error for backward compatibility. DO NOT USE WITHOUT EXPLICIT CONFIRMATION that it's desired.                                                                                                                                                                                                          | ❌ NEVER use without explicit permission |
| logger.error | 11 🟣        | POSTS TO SLACK - HIGHEST PRIORITY emergency notification for CRITICAL system failures. Triggers IMMEDIATE alerts to ALL engineers including out-of-hours notifications. Reserved for catastrophic errors affecting many users or threatening system stability. Routes to same function as logger.error for backward compatibility. DO NOT USE WITHOUT EXPLICIT CONFIRMATION that it's desired.                                                                      | ❌ NEVER use without explicit permission |

### 3. Specialized Logger Methods (Use-Case Specific)

These specialized logger methods are designed for specific use cases and also notify the team via Slack.

| Method               | Significance | Usage                                                                                                                                                                                                                                                                                                                                                                                         | Agent Usage Policy                       |
| -------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| logger.event         | 6 🟠         | POSTS TO SLACK - Sends detailed business event data to dedicated analytics channel. SEEN BY ENTIRE ANALYTICS TEAM. Triggers workflow automations and business metrics. Invalid events can trigger false business alerts. Previously caused 4+ hours of cleanup work when misused. DO NOT USE WITHOUT EXPLICIT CONFIRMATION that it's desired.                                                 | ❌ NEVER use without explicit permission |
| logger.newUserLog    | 6 🟠         | POSTS TO SLACK - Broadcasts new user registration data to customer success team. SEEN BY CUSTOMER SUCCESS AND MANAGEMENT TEAM. May trigger onboarding workflows. Incorrect usage can cause customer confusion and support tickets. Previously caused 4+ hours of cleanup work when misused. DO NOT USE WITHOUT EXPLICIT CONFIRMATION that it's desired.                                       | ❌ NEVER use without explicit permission |
| logger.warnThrottled | 8 🔴         | POSTS TO SLACK - Similar to logger.warn but with rate limiting to prevent channel flooding. Still notifies ENTIRE DEVELOPMENT TEAM but groups identical warnings. MULTIPLE team notifications occur but at reduced frequency. Every team member's Slack still lights up. Previously caused 4+ hours of cleanup work when misused. DO NOT USE WITHOUT EXPLICIT CONFIRMATION that it's desired. | ❌ NEVER use without explicit permission |

| Error Code | File Path                                                              | Error Description                    | Location                                                         | Current Level | Target Level |
| ---------- | ---------------------------------------------------------------------- | ------------------------------------ | ---------------------------------------------------------------- | ------------- | ------------ |
| 1001       | middlewares/errorHandler.js                                            | Network connectivity error           | Network error detection block                                    | 8 🔴          | TBD          |
| 1002       | controllers/subscription/customer.subscription.created.js              | User not found for stripeCustomerId  | Find customer in newStripeSubscriptionCreated function           | 8 🔴          | TBD          |
| 1003       | controllers/subscription/customer.subscription.created.js              | Could not save trial plan update     | After retry failures in saveWithRetry function                   | 8 🔴          | TBD          |
| 1004       | controllers/subscription/customer.subscription.created.js              | Unexpected error in trial processing | Error catch block in trial plan update                           | 8 🔴          | TBD          |
| 1005       | controllers/subscription/customer.subscription.created.js              | Failed trial initialization tracking | Trial tracking error handler                                     | 8 🔴          | TBD          |
| 1006       | controllers/subscription/customer.subscription.created.js              | Failed to save trial dates           | Error callback in saveWithRetry for updateSubscriptionTrialDates | 8 🔴          | TBD          |
| 1007       | controllers/subscription/customer.subscription.created.js              | Could not save trial dates           | After retry failures in updateSubscriptionTrialDates             | 8 🔴          | TBD          |
| 1008       | controllers/subscription/customer.subscription.created.js              | Failed to update trial dates         | Main error catch in updateSubscriptionTrialDates                 | 8 🔴          | TBD          |
| 1020       | controllers / authController.js                                        | User registered without password     | Registration function where password is checked                  | 7 🟠          | TBD          |
| 1021       | controllers / authController.js                                        | MongoDB password check failed        | Password match validation function                               | 7 🟠          | TBD          |
| 1022       | controllers / authController.js                                        | Error determining funnel stage       | Try-catch block in funnel stage determination                    | 7 🟠          | TBD          |
| 1023       | utils / stripeLogger.js                                                | Trial period detection error         | CC trial check error handling                                    | 7 🟠          | TBD          |
| 1024       | subscriptions-and-stripe-webhooks / persistence / userPersistence.js   | User update failed - user not found  | During user update operation                                     | 7 🟠          | TBD          |
| 1025       | subscriptions-and-stripe-webhooks / entity-resolvers / userResolver.js | Failed to resolve user               | When user identifiers don't match any user                       | 7 🟠          | TBD          |
| 1026       | utils / SlackNotifier.js                                               | Slack notification send error        | Error handler in sendMessage function                            | 7 🟠          | TBD          |
| 1027       | utils / meta / facebookEvents.js                                       | No formatted FBC available for user  | When attempting to format Facebook click ID                      | 7 🟠          | TBD          |
| 1028       | utils / meta / trialInitialized.js                                     | CC trial check error                 | Error handling for trial check with fallback to stripeCustomerId | 7 🟠          | TBD          |
| 1029       | utils / networkResilience.js                                           | API retry max attempts exceeded      | When all retry attempts for a network operation fail             | 7 🟠          | TBD          |
| 1030       | utils / logger.js                                                      | Failed Slack notification delivery   | Error handler for Slack message transport                        | 7 🟠          | TBD          |
| 1031       | utils / ai / llmRunner.js                                              | Invalid input parameter to LLM       | When validating input parameters for LLM call                    | 7 🟠          | TBD          |
| 1032       | utils / ai / llmRunner.js                                              | Parameter confusion in LLM call      | When directory path is passed as conversationId                  | 7 🟠          | TBD          |
| 1033       | utils / ai / llmRunner.js                                              | Format query failed in LLM runner    | When attempting to format LLM query                              | 7 🟠          | TBD          |
| 1034       | utils / subscription-cancellation-handler.js                           | Missing subscription resolver        | When subscription resolver module not available                  | 7 🟠          | TBD          |
| 1035       | controllers / admin / userStatusController.js                          | User account login issues detected   | When system detects problems with user's login capabilities      | 7 🟠          | TBD          |
| 1036       | subscriptions-and-stripe-webhooks / entity-operations / subscriptionOperations.js | Subscription record not found          | When trying to update trial dates for non-existent subscription  | 7 🟠          | TBD          |
| 1037       | controllers / subscription / customer.subscription.created.js          | MongoDB connection not ready         | When attempting to save during poor database connection          | 7 🟠          | TBD          |
| 1038       | controllers / subscription / customer.subscription.created.js          | Failed save attempt                  | When document save operation fails and retry is attempted        | 7 🟠          | TBD          |
| 1039       | controllers / subscription / customer.subscription.created.js          | Trial dates update retry             | When attempting to retry saving trial dates after failure        | 7 🟠          | TBD          |
| 1040       | controllers / subscription / customer.subscription.created.js          | No subscription record for trial     | When subscription record not found for trial dates update        | 7 🟠          | TBD          |
| 1041       | controllers / authController.js                                        | Login error for email                | Authentication failed during login process                        | 10 🔴         | TBD          |
| 1042       | utils / SlackNotifier.js                                               | Fallback error message delivery      | When primary error logging fails, fallback handling in sendErrorMessage | 10 🔴       | TBD          |
| 1043       | utils / stripeLogger.js                                                | Critical checkout failure            | When payment or subscription checkout process fails                | 10 🔴         | TBD          |
| 1044       | utils / stripeLogger.js                                                | Stripe customer retrieval error      | When retrieving a customer from Stripe fails                       | 10 🔴         | TBD          |
| 1045       | utils / stripeLogger.js                                                | Stripe session customer error        | Customer record issue during session creation                       | 10 🔴         | TBD          |
| 1046       | utils / stripeLogger.js                                                | Webhook signature verification error | When Stripe webhook signature validation fails                      | 10 🔴         | TBD          |
| 1047       | utils / stripeLogger.js                                                | Stripe event handling error          | When processing webhook events from Stripe                          | 10 🔴         | TBD          |
| 1048       | utils / subscription-cancellation-handler.js                           | Subscription record update error     | When updating a subscription record during cancellation processing  | 10 🔴         | TBD          |
| 1049       | utils / subscription-cancellation-handler.js                           | Fallback subscription update error   | When fallback update method for subscription record fails           | 10 🔴         | TBD          |
| 1050       | utils / subscription-cancellation-handler.js                           | Subscription status resolution error | When resolving user's subscription status fails                     | 10 🔴         | TBD          |
| 1051       | utils / subscription-cancellation-handler.js                           | Subscription cancellation error      | General error during subscription cancellation processing           | 10 🔴         | TBD          |
| 1052       | controllers / authController.js                                        | Firebase user creation error         | When creating a Firebase user record fails                          | 10 🔴         | TBD          |
| 1053       | controllers / authController.js                                        | Password update error                | When updating user password fails                                   | 10 🔴         | TBD          |
| 1054       | controllers / authController.js                                        | Firebase user creation error         | When creating a Firebase user with password provider fails          | 10 🔴         | TBD          |
| 1055       | controllers / authController.js                                        | Firebase user retrieval error        | When retrieving a Firebase user record fails                        | 10 🔴         | TBD          |
| 1056       | controllers / authController.js                                        | Password provider update error       | When adding password provider to existing Firebase user fails       | 10 🔴         | TBD          |
| 1057       | controllers / authController.js                                        | General password provider error      | Unspecified error when enabling password provider                   | 10 🔴         | TBD          |
| 1058       | controllers / authController.js                                        | Email status check error             | Error checking if email exists and needs authentication setup       | 10 🔴         | TBD          |
| 1059       | controllers / authController.js                                        | Firebase password update error       | When updating Firebase user with password fails                     | 10 🔴         | TBD          |
| 1060       | controllers / authController.js                                        | Firebase user creation error         | When creating Firebase user for existing MongoDB user               | 10 🔴         | TBD          |
| 1061       | controllers / authController.js                                        | Firebase registration error          | When creating a Firebase user during non-payment registration fails | 8 🟠          | TBD          |
| 1062       | controllers / authController.js                                        | MongoDB registration error           | When saving MongoDB user during non-payment registration fails      | 8 🟠          | TBD          |
| 1063       | controllers / authController.js                                        | Registration process error           | Unexpected error during non-payment registration process            | 8 🟠          | TBD          |
| 1064       | controllers / authController.js                                        | User profile lookup error            | When retrieving user profile with valid token but user not found    | 8 🟠          | TBD          |
| 1065       | controllers / authController.js                                        | Subscription lookup error            | When retrieving subscription data for user profile                  | 8 🟠          | TBD          |
| 1066       | controllers / authController.js                                        | Profile request error                | Unexpected error during profile request processing                  | 8 🟠          | TBD          |
| 1067       | controllers / authController.js                                        | Trial check user lookup error        | User not found when checking trial status                           | 8 🟠          | TBD          |
| 1068       | controllers / authController.js                                        | Trial status determination error     | Error when determining trial eligibility                            | 8 🟠          | TBD          |
| 1069       | controllers / authController.js                                        | Trial check process error            | Unexpected error during trial status check process                  | 8 🟠          | TBD          |
| 1070       | controllers / subscription / stripeController.js                       | Checkout session creation error      | When creating a subscription checkout session fails                 | 10 🔴         | TBD          |
| 1071       | controllers / subscription / stripeController.js                       | Public checkout session error        | When creating a public checkout session fails                       | 10 🔴         | TBD          |
| 1072       | controllers / subscription / stripeController.js                       | Email checkout session error         | When creating an email-based checkout session fails                 | 10 🔴         | TBD          |
| 1073       | controllers / subscription / planController.js                         | Plan list retrieval error            | When retrieving subscription plans for frontend display fails       | 8 🟠          | TBD          |
| 1074       | controllers / subscription / planController.js                         | Plan not found error                 | When specific requested plan ID cannot be found                     | 8 🟠          | TBD          |
| 1075       | controllers / subscription / planController.js                         | Plan detail retrieval error          | When retrieving a specific plan's details fails                     | 8 🟠          | TBD          |
| 1076       | controllers / subscription / planController.js                         | Admin plans retrieval error          | When admin cannot retrieve all plans for management                 | 8 🟠          | TBD          |
| 1077       | controllers / subscription / planController.js                         | Admin plan update not found          | When admin tries to update a non-existent plan                      | 8 🟠          | TBD          |
| 1078       | controllers / subscription / planController.js                         | Admin plan update error              | When admin plan update operation fails                              | 8 🟠          | TBD          |
| 1079       | controllers / subscription / planController.js                         | Admin plan delete not found          | When admin tries to delete a non-existent plan                      | 8 🟠          | TBD          |
| 1080       | controllers / subscription / planController.js                         | Admin plan delete error              | When admin plan deletion operation fails                            | 8 🟠          | TBD          |
| 1081       | controllers / subscription / planController.js                         | Admin plan creation error            | When admin plan creation operation fails                            | 8 🟠          | TBD          |
| 1082       | middlewares / auth.js                                                  | JWT token validation error           | When JWT token validation fails (expired, malformed, etc.) for protected endpoint access | 10 🔴      | 8 🟠         |
| 1083       | middlewares / auth.js                                                  | Auth header validation error         | Covers three specific cases now distinguished with error_code: AUTH_HEADER_MISSING, AUTH_FORMAT_INVALID, AUTH_TOKEN_EMPTY | 10 🔴 | 8 🟠  |
| 1084       | controllers / conversationsController.js                               | Conversation creation error          | When user fails to create a new conversation                         | 10 🔴         | TBD          |
| 1085       | controllers / conversationsController.js                               | Message creation error               | When user fails to add a message to an existing conversation         | 10 🔴         | TBD          |
| 1086       | controllers / conversationsController.js                               | Practice session creation error      | When user fails to create a practice session                         | 10 🔴         | TBD          |
| 1087       | controllers / subscription / stripeController.js                       | Plan not found for payment intent    | When creating a payment intent with invalid plan ID                  | 10 🔴         | TBD          |
| 1088       | controllers / subscription / stripeController.js                       | Payment intent creation error        | When Stripe API call to create payment intent fails                  | 10 🔴         | TBD          |
| 1089       | controllers / subscription / stripeController.js                       | Plan not found for payment update    | When updating a payment intent with email but plan ID is invalid     | 10 🔴         | TBD          |
| 1090       | controllers / subscription / stripeController.js                       | Payment intent email update error    | When Stripe API call to update payment intent with email fails       | 10 🔴         | TBD          |
| 1091       | controllers / subscription / stripeController.js                       | Checkout session completion error   | When processing checkout.session.completed webhook fails            | 10 🔴         | TBD          |
| 1092       | middlewares / auth.js                                                  | Subscription access check error     | When verification of user's subscription access fails during authentication | 8 🟠      | 6 🟠         |
| 1093       | middlewares / auth.js                                                  | Trial status verification error     | When checking trial eligibility during authentication fails          | 8 🟠          | 6 🟠         |
| 1094       | middlewares / auth.js                                                  | Missing user ID in token            | When a valid JWT token lacks any recognizable user ID property       | 8 🟠          | 6 🟠         |
| 1095       | controllers / subscription / invoice.payment_failed.js                 | Failed payment processing            | When user's payment fails and we can't identify the user         | 10 🔴         | 8 🟠         |

<!-- When adding new codes, please follow this format and increment the last code by 1 -->
