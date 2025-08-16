# Issue Tracking Map

## Related Resources
- [IXCoach API Changelog](../changelog/index.md)
- [IXCoach API Changelog Map](../changelog/Changelog-Map.md)
- [Map To All Failing Tests](../tests/TEST-MAP.md)
- [Coverage Map of Error Logs](./error_logging_audit.md)

This document provides a comprehensive overview of all issues tracked in the ixcoach-api repository, organized by category and status.

## Issue Status Overview

| Status                    | Count | Percentage | Description                                                                     |
| ------------------------- | ----- | ---------- | ------------------------------------------------------------------------------- |
| 🔵 Deployed/Staging       | 15    | 17%        | Issue has been fixed, tested, and deployed to production or in staging          |
| 🟢 Fixed                  | 11    | 12%        | Issue has been resolved with code changes that have been tested                 |
| 🟡 In Progress            | 25    | 28%        | Issue is actively being addressed with implementation ongoing                   |
| 🟠 Identified             | 26    | 29%        | Issue has been documented but implementation has not started                    |
| 🟠 Root Cause Analysis Complete | 0     | 0%         | Issue has been thoroughly analyzed with exact cause identified                 |
| 🟠 Partially Done         | 4     | 4%         | Issue has been partially implemented but key components remain unfinished       |
| 🔴 Todo                   | 7     | 8%         | Issue has been identified as necessary but work has not yet begun               |
| 🔴 Critical               | 4     | 4%         | High-impact issue affecting key user journeys that requires immediate attention |

## Issue Distribution By Category

### Authentication & Login (31 issues)

| Status            | Issue                                                                                                                                      | Intent                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| 🔵 Deployed       | [Firebase Provider Issue](./Sign_in_bugs_Issue_Apr_2_2025/completed/FIREBASE_PROVIDER_ISSUE_COMPLETED.md)                                  | Aimed to add missing password authentication methods for users attempting to log in with email/password                |
| 🔵 Deployed       | [Cross-Domain Auth Cookie Fix](../changelog/2025-04-11-cross-domain-auth-cookie-fix.md)                                                    | Aimed to standardize cookie domain settings for seamless user authentication across subdomains                         |
| 🔵 Deployed       | [WebView Authentication Timeout Fix](../../../gptcoach2/docs/issues/WEBVIEW_AUTH_TIMEOUT_FIX.md)                                           | Aimed to prevent infinite loading screens for users authenticating through YouTube WebView                             |
| 🟢 Fixed          | [Paid Users Free Treatment](./login/paid-users-free-treatment.md)                                                                          | Aimed to correctly identify and assign premium trial status to users who sign up with credit cards                     |
| 🟠 Identified     | [Canonical Cookie System Initiative](./Initiative_Canonical_Cookie_System.md)                                                              | Aimed to implement a unified, consistent cookie system for seamless authentication across all platforms and contexts   |
| 🟡 In Progress    | [Auth Cleaner Investigation](./2025-04-16-auth-cleaner-investigation.md)                                                                   | Aimed to identify potential implementation issues affecting users moving between landing and IXC apps                  |
| 🟡 In Progress    | [Cross-Domain Authentication Persistence](../../../gptcoach2/docs/issues/CROSS_DOMAIN_AUTH_PERSISTENCE_ISSUE.md)                           | Aimed to ensure session persistence for users navigating between landing page and main app                             |
| 🟡 In Progress    | [One-Click Login Issue](./fix_one_click_confirmed_issue.md)                                                                                | Aimed to fix misleading "check email" messages for users requesting login links for non-existent accounts              |
| 🟡 In Progress    | [Email Normalization Solution](./Sign_in_bugs_Issue_Apr_2_2025/agent_review/2_solution_design/EMAIL_NORMALIZATION_SOLUTION.md)             | Aimed to standardize email handling across auth flows when users log in with different email casing                    |
| 🟡 In Progress    | [WebView Authentication Failure](./Sign_in_bugs_Issue_Apr_2_2025/agent_review/1_issue_analysis/WEBVIEW_AUTHENTICATION_FAILURE_ANALYSIS.md) | Aimed to address authentication limitations for users accessing through in-app browsers                                |
| 🟢 Fixed          | [Missing MongoDB User Creation](./login/missing-mongo-user-creation.md)                                                                    | Aimed to create MongoDB user records for valid Stripe customers who cannot access the platform                         |
| 🟡 In Progress    | [Password Required Redirect](./login/password-required-redirect.md)                                                                        | Aimed to guide users without passwords to the registration page to set up password authentication                      |
| 🟡 In Progress    | [Payment Authentication Completion](./login/payment-authentication-completion.md)                                                          | Aimed to ensure users who purchase subscriptions can establish authentication methods even if they close their browser |
| 🟢 Fixed          | [Google Auth WebView Method](./login/google-auth-webview-method.md)                                                                        | Aimed to recommend Google as login method for users who previously used Google auth in WebView                         |
| 🟡 In Progress    | [CLI Password Reset Tool](./login/cli-password-reset.md)                                                                                   | Aimed to provide support teams with secure method to help users reset passwords                                        |
| 🟠 Identified     | [Google Auth Detection Issue](./GoogleAuthDetectionIssue/GOOGLE_AUTH_DETECTION_ISSUE.md)                                                   | Aimed to improve detection of Google auth sources when users authenticate via Google from different devices            |
| 🟠 Identified     | [Session State Persistence Issue](./Sign_in_bugs_Issue_Apr_2_2025/human_logs/SESSION_STATE_PERSISTENCE_ISSUE.md)                           | Aimed to resolve login state conflicts for users switching between multiple accounts                                   |
| 🟠 Partially Done | [Block Existing Password Registration](./login/block-existing-password-registration.md)                                                    | Aimed to prevent users with existing passwords from accidentally overwriting them during registration                  |
| 🟠 Partially Done | [Isolated Login Tests](./login/isolated-login-tests.md)                                                                                    | Aimed to create isolated test suites for different authentication scenarios using dedicated Claude agents              |
| 🔴 Todo           | [Component-Based Login Messages](./login/component-based-login-messages.md)                                                                | Aimed to replace toast notifications with persistent component-based messages for critical authentication states       |
| 🔴 Todo           | [Password Reset Helper](./login/password-reset-helper.md)                                                                                  | Aimed to display helper window with password reset option when users enter incorrect passwords                         |
| 🔴 Todo           | [Hidden Email Password Field](./login/hidden-email-password-field.md)                                                                      | Aimed to fix bug where email/password input fields are not visible by default on login page                            |
| 🔴 Todo           | [Centralized Login State](./login/centralized-login-state.md)                                                                              | Aimed to implement single-source login state handling for consistent authentication behavior                           |
| 🔴 Maybe          | [Redux Login State](./login/redux-login-state.md)                                                                                          | Aimed to add Redux or equivalent state management to make login state machine more predictable                         |

### Subscription & Payment (16 issues)

| Status         | Issue                                                                                                 | Intent                                                                                                     |
| -------------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| 🔵 Deployed/Staging | [Quarterly Plan Subscribers Issue](./2025-04-14-quarterly-plan-subscribers-issue.md)             | Aimed to address incorrect plan assignment preventing paid quarterly users from accessing premium features [Changelog](../changelog/2025-04-15-plan-resolver-implementation.md) |
| 🔵 Deployed/Staging | [Webhook Issues Index](./2025-04-16-webhook-issues-index.md)                                      | Aimed to address multiple Stripe webhook data extraction failures affecting subscription management [Changelog](./webhook-issues/2025-04-16-stripe-webhook-extraction-completion.md) |
| 🔵 Deployed/Staging | [Premium Access Delay Fix](../changelog/2025-04-16-premium-access-delay-fix.md)                   | Aimed to eliminate delays between payment completion and premium access activation for all users |
| 🔵 Deployed/Staging | [Trial User CC Plan Access Issue Fix](../changelog/2025-04-16-trial-user-cc-plan-access-issue-fix.md) | Aimed to fix intermittent loss of premium access for users with credit cards after login/logout cycles |
| 🔵 Deployed/Staging | [Plan Resolver Implementation](../changelog/2025-04-15-plan-resolver-implementation.md)           | Aimed to implement a canonical entry point for plan resolution with Stripe synchronization |
| 🔵 Deployed    | [Plan Resolver Refactoring](../changelog/2025-04-11-plan-resolver-refactoring.md)                     | Aimed to reorganize plan resolver architecture to improve backend stability for plan resolution [Changelog](../changelog/2025-04-11-plan-resolver-refactoring.md) |
| 🔵 Deployed    | [One-Click Login Plan Assignment Fix](../changelog/2025-04-11-one-click-login-plan-assignment-fix.md) | Aimed to fix plan assignment for users completing payment through one-click login flow [Changelog](../changelog/2025-04-11-one-click-login-plan-assignment-fix.md) |
| 🔵 Deployed    | [Immediate Trial Plan Assignment](../changelog/2025-04-10-immediate-trial-plan-assignment.md)         | Aimed to improve onboarding experience for new trial users accessing premium features [Changelog](../changelog/2025-04-10-immediate-trial-plan-assignment.md) |
| 🟢 Fixed       | [Stripe Webhook Race Condition Fix](../changelog/2025-04-16-stripe-webhook-race-condition-fix.md)     | Aimed to ensure database records are correctly updated when Stripe webhooks arrive before user creation |
| 🟡 In Progress | [Subscription Status Resolver Tests](../changelog/2025-04-10-subscription-status-resolver-tests.md)   | Aimed to validate subscription status resolution during user subscription status transitions [Changelog](../changelog/2025-04-10-subscription-status-resolver-tests.md) |
| 🟡 In Progress | [Payment Error Recovery](./api_test_issues/PAYMENT_ERROR_RECOVERY.md)                                 | Aimed to improve guidance for users encountering credit card declines or payment failures                  |
| 🟡 In Progress | [Trial Tracking Tests Logging Fix](./2025-04-07-trial-tracking-tests-logging-fix.md)                   | Aimed to improve trial period logging for trial user journey analytics                                     |

### Error Handling & Monitoring (11 issues)

| Status         | Issue                                                                                               | Intent                                                                                 |
| -------------- | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 🔵 Deployed    | [Error Code Registry Implementation](../changelog/2025-04-11-error-code-registry-implementation.md) | Aimed to standardize error handling to provide consistent error messages for users [Changelog](../changelog/2025-04-11-error-code-registry-implementation.md) |
| 🔵 Deployed    | [Error Code Registry Update](../changelog/2025-04-10-error-code-registry-update.md)                 | Aimed to expand error code documentation for more specific error guidance to users [Changelog](../changelog/2025-04-10-error-code-registry-update.md) |
| 🟢 Fixed       | [Stripe Webhook Race Condition Fix](./2025-04-16-stripe-webhook-race-condition-fix.md)              | Aimed to ensure database records are correctly updated when Stripe webhooks arrive before user creation [Changelog](../changelog/2025-04-16-stripe-webhook-race-condition-fix.md) |
| 🟡 In Progress | [Error Logging Audit](./error_logging_audit.md) | Aimed to ensure all mission-critical API endpoints have proper error logging with appropriate severity levels |
| 🟡 In Progress | [Webhook Validation Issues](./test_failures/WEBHOOK_VALIDATION_ISSUES.md)                           | Aimed to improve webhook data validation in backend processing of subscription events  |
| 🟡 In Progress | [Important Warning Logs](../WARNING_LOG_TESTS.md)                                                   | Aimed to standardize warning log format to improve support team issue diagnosis        |
| 🟡 In Progress | [Aggregate Login Cookie Issue Report](./aggregate_login_cookie_issue_report.md)                     | Aimed to document cookie inconsistencies affecting user authentication across domains  |
| 🟠 Identified  | [Subscription Access Logging Mismatch](./2025-04-07-subscription-access-logging-mismatch.md)       | Aimed to fix subscription access logging for subscription utilization analytics        |

### Webhook Issues (6 issues)

| Status         | Issue                                                                                                   | Intent                                                                                                                                                                                                                                                                                                                                   |
| -------------- | ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🔵 Deployed/Staging | [Stripe Webhook Data Extraction Centralization](./webhook-issues/2025-04-16-stripe-webhook-extraction-completion.md) | Aimed to centralize and standardize webhook data extraction across all Stripe webhook handlers to eliminate errors like "stripeDataObject.lines is null or undefined" and "Failed to extract stripeProductId from webhook data". Implementation includes a centralized extractor with comprehensive test coverage.                         |
| 🔵 Deployed/Staging | [Missing User ID Extraction Issue](./webhook-issues/2025-04-16-Missing-User-ID-Extraction-Issue.md)     | Aimed to improve user ID extraction in webhook processing. This issue has been resolved with the centralized webhook data extractor implementation. |
| 🔵 Deployed/Staging | [Stripe Product ID Extraction Issue](./webhook-issues/2025-04-16-Stripe-Product-ID-Extraction-Issue.md) | Aimed to address the "stripeDataObject.lines is null or undefined - Failed to extract stripeProductId from webhook data" error in invoice.payment_succeeded.js. Fixed with the centralized webhook data extractor implementation. |
| 🔵 Deployed/Staging | [Incomplete Payment Failed Data Issue](./webhook-issues/2025-04-16-Incomplete-Payment-Failed-Data-Issue.md) | Aimed to resolve the "stripeCustomerId: undefined, email: undefined, clientReferenceId: undefined, eventType: 'invoice.payment_failed'" error. Fixed with robust data extraction in the centralized extractor. |
| 🟢 Fixed       | [Malformed Webhook Data Handling](../tests/reports/T_5072_malformed_webhook_data.md) | Aimed to standardize error handling in webhook data extractor to consistently return `null` for error cases instead of `undefined`, ensuring paid users receive correct plan assignments even when webhook data is malformed. |
| 🟢 Fixed       | [Stripe API Error Resilience](../tests/reports/T_5073_stripe_api_errors.md) | Aimed to implement robust error categorization, retry logic, and graceful degradation for Stripe API failures to maintain subscription processing during Stripe API outages. |

### Database & Infrastructure (8 issues)

| Status         | Issue                                                                           | Intent                                                                                        |
| -------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| 🟢 Fixed       | [Mongoose Timeout Issue](./test_failures/MONGOOSE_TIMEOUT_ISSUE.md)             | Aimed to fix database timeout handling to prevent timeouts during high load                   |
| 🟡 In Progress | [Database Connection Issue](./test_failures/DATABASE_CONNECTION_ISSUE.md)       | Aimed to resolve connection stability issues affecting backend reliability for all operations |
| 🟡 In Progress | [Redis Connection Issues](./test_failures/REDIS_CONNECTION_ISSUES.md)           | Aimed to improve Redis connection resilience for session persistence and caching              |
| 🟠 Identified  | [MongoDB Firebase Sync Issue](./2025-04-07-mongodb-firebase-sync-issue.md)       | Aimed to fix data synchronization affecting user data consistency across services             |
| 🟠 Identified  | [Firebase Sync Database Issue](./test_failures/FIREBASE_SYNC_DATABASE_ISSUE.md) | Aimed to fix Firebase data synchronization for user profile consistency                       |

### User Experience & Interface (5 issues)

| Status         | Issue                                                                                                                         | Intent                                                                              |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 🔵 Deployed    | [Enhanced User Lookup Status Management](../changelog/2025-04-12-enhanced-user-lookup-status-management.md)                   | Aimed to improve user lookup operations for admin tools supporting customer service [Changelog](../changelog/2025-04-12-enhanced-user-lookup-status-management.md) |
| 🔵 Deployed    | [Account Management CLI Enhancements](../account-management/docs/changelog/2025-04-11-account-management-cli-enhancements.md) | Aimed to add diagnostic and operational tools for admin subscription management [Changelog](../account-management/docs/changelog/2025-04-11-account-management-cli-enhancements.md) |
| 🔴 Critical    | [Apple Email Blocking Issue](./2025-04-07-apple-email-blocking-issue.md)                                                     | Aimed to fix email delivery to Apple domains for users with Apple email addresses   |
| 🟡 In Progress | [User Engagement Menu Helper Text](./2025-04-16-user-engagement-menu-helper-text.md)                                         | Aimed to improve CLI tool guidance for admin console usability                      |

### Marketing & Analytics (5 issues)

| Status         | Issue                                                                                   | Intent                                                                          |
| -------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| 🔴 Critical    | [Facebook Conversion Tracking](./2025-04-14-facebook-conversion-tracking-comparison.md) | Aimed to restore working conversion tracking for marketing attribution from ads |
| 🟡 In Progress | [Email Notification Reliability](./api_test_issues/EMAIL_NOTIFICATION_RELIABILITY.md)   | Aimed to ensure timely email delivery for users receiving system notifications  |

## Cookie System Initiative

The following tasks are part of the [Canonical Cookie System Initiative](./Initiative_Canonical_Cookie_System.md) to address authentication issues:

| Status       | Task                                                                                                                                  | Intent                                                                                              | Related Issues |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|----------------|
| 🟠 Identified | [Login Duration Audit](./2025-04-17-complete-login-duration-audit-across-all-cookie-write-paths.md)                                   | Aimed to standardize all cookie durations to 30 days to eliminate random logouts                    | [Google Auth Detection Issue](./GoogleAuthDetectionIssue/GOOGLE_AUTH_DETECTION_ISSUE.md), [Connectivity Resilience Plan](../initiatives/CONNECTIVITY_RESILIENCE_PLAN.md) |
| 🟠 Identified | [Clean Up Duration Fixes](./2025-04-17-validate-and-clean-up-cookie-duration-set-by-prior-fixes.md)                                   | Aimed to remove redundant cookies from previous fixes to reduce cookie conflicts                    | [Session State Persistence Issue](./Sign_in_bugs_Issue_Apr_2_2025/human_logs/SESSION_STATE_PERSISTENCE_ISSUE.md) |
| 🟠 Identified | [Create Cookie Resolver](./2025-04-17-create-resolver-to-consolidate-all-possible-cookie-values.md)                                   | Aimed to implement deterministic cookie resolution system to address authentication conflicts      | [Cross-Platform Authentication](./api_test_issues/CROSS_PLATFORM_AUTHENTICATION.md) |
| 🟠 Identified | [Multi-Domain Cookie Reader](./2025-04-17-design-modular-resolver-for-reading-from-multiple-cookie-domains.md)                        | Aimed to read cookies across all domains for seamless cross-domain authentication                  | [Cross-Domain Authentication Persistence](../../../gptcoach2/docs/issues/CROSS_DOMAIN_AUTH_PERSISTENCE_ISSUE.md) |
| 🟠 Identified | [Cookie Collision Detection](./2025-04-17-detect-and-handle-auth-collisions-from-conflicting-cookies.md)                              | Aimed to detect and resolve conflicting user identities to prevent hybrid cookie states            | [Payment Authentication Flow](./payment-authentication-flow/README.md) |
| 🟠 Identified | [WebView Cookie Fixes](./2025-04-17-fully-resolve-cookie-write-and-read-inconsistencies-in-webview.md)                               | Aimed to fix WebView-specific cookie issues so social media users stay logged in                   | [WebView Authentication Failure](./Sign_in_bugs_Issue_Apr_2_2025/agent_review/1_issue_analysis/WEBVIEW_AUTHENTICATION_FAILURE_ANALYSIS.md) |
| 🟠 Identified | [IXC Cookie Integration](./2025-04-17-confirm-ixc-flows-are-receiving-canonical-cookies.md)                                           | Aimed to ensure ixc.im domain compatibility for consistent IXC experiences                         | [Component-Based Login Messages](./login/component-based-login-messages.md) |
| 🟠 Identified | [Critical Cookie Resolution](./2025-04-17-elevate-broken-cookie-resolution-to-blocker-status.md)                                      | Aimed to make cookie failures visible not silent for clear recovery from authentication failures   | [Webhook Route Issue](./test_failures/WEBHOOK_ROUTE_ISSUE.md) |
| 🟠 Identified | [Centralize Cookie Management](./2025-04-17-centralize-cookie-setting-and-reading-to-unify-auth-handling.md)                          | Aimed to complete migration to central cookie manager for unified authentication                   | [Cross-Domain Auth Cookie Fix](../changelog/2025-04-11-cross-domain-auth-cookie-fix.md) |
| 🟠 Identified | [Cross-Context Validation](./2025-04-17-evaluate-and-fix-cookie-auth-persistence-across-contexts.md)                                  | Aimed to perform final testing across all contexts for verified authentication reliability         | [Aggregate Login Cookie Issue Report](./aggregate_login_cookie_issue_report.md) |

## Critical Path User Journeys

The following user journeys are currently impacted by multiple issues:

1. **Quarterly Plan Subscription Journey** (🔵 Deployed/Staging)

   - Multiple issues affecting quarterly plan subscribers have been fixed
   - Implementation includes a canonical plan resolver entry point
   - Plan resolver now syncs with Stripe automatically
   - This fix is in staging and awaiting final human approval before deployment

2. **Cross-Domain Authentication Journey** (🟡 In Progress)

   - Several issues affecting users moving between landing pages and main application
   - Cookie handling, WebView authentication, and session persistence all involved
   - Progress made with the Cross-Domain Auth Cookie Fix (2025-04-11) implementing a dual-cookie pattern
   - Canonical Cookie System Initiative identified to comprehensively address remaining cookie issues

3. **Payment Authentication Completion Journey** (🔵 Deployed/Staging)

   - Fixed issues where users purchasing with email only via Stripe checkout lost access after closing browser
   - Enhanced email flow and authentication handlers implemented
   - Special handling for Outlook email users to prevent security warnings
   - Single source of truth pattern enforced for consistent plan resolution

4. **Stripe Webhook Processing** (🔵 Deployed/Staging)

   - Issues with webhook processing affecting subscription activation have been fixed
   - Centralized webhook data extractor implemented to handle all webhook formats consistently
   - Improved error handling and resilience for webhook processing
   - Race condition handling for webhooks arriving before user creation
   - This fix is in staging and awaiting final human approval before deployment

5. **Trial Conversion Journey** (🔵 Deployed/Staging)

   - Fixed issue where users with credit cards were treated as free users during trial
   - Fixed intermittent loss of premium access after login/logout cycles
   - Eliminated delays between payment completion and premium access activation
   - Smooth transition from trial to paid status maintains consistent user experience

6. **Marketing Attribution Journey** (🔴 Critical)
   - Facebook conversion tracking broken, affecting marketing ROI calculation
   - Implementation comparison shows vital data not being sent to Facebook
   - Needs immediate attention to restore proper attribution

7. **Apple Email Delivery** (🔴 Critical)
   - Critical issue preventing email delivery to users with Apple email addresses
   - Impacts login links, password resets, and notifications for Apple users
   - Requires immediate attention to ensure these users can authenticate properly

## Recent Progress Highlights

1. **Plan Resolver Implementation** - Comprehensive implementation of a canonical entry point for plan resolution with Stripe synchronization
2. **Webhook Processing Improvements** - Centralized webhook data extractor implemented to handle all webhook formats consistently
3. **Authentication Improvements** - Multiple fixes for WebView and cross-domain auth
4. **Trial User Experience Enhancement** - Fixed multiple issues affecting trial users with credit cards
5. **Payment Experience Optimization** - Eliminated delays between payment completion and premium access activation
6. **Stripe Webhook Race Condition Fix** - Added handling for webhooks arriving before user creation
7. **Error Standardization** - Implementation of centralized error code registry
8. **Admin Tools** - Enhanced CLI tools for subscription management

## Documentation Initiatives

| Status         | Initiative                                                                         | Intent                                                                       |
| -------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 🟡 In Progress | [Function Intent Documentation](./FUNCTION-INTENT-INDEX-MAP.md)                    | Aimed to document critical backend functions using "When X, this aims to Y" pattern to clarify business intent |

## Next Steps Recommendations

Based on the issue distribution, these areas should be prioritized:

1. **Fix Apple Email Blocking Issue** - Critical for users with Apple email addresses to receive login links and notifications
2. **Restore Facebook Conversion Tracking** - Critical for marketing ROI calculation
3. **Complete Cross-Domain Authentication Fixes** - Ensure seamless user experience between landing pages and main application
4. **Deploy Plan Resolver and Webhook Processing Improvements** - Move fixes from staging to production
5. **Improve Email Notification Reliability** - Ensure timely delivery of system notifications
6. **Complete Function Intent Documentation** - Continue documenting critical backend functions