# Function Intent Documentation Map

## Related Resources
- [IXCoach API Changelog](../changelog/index.md)
- [IXCoach API Documentation](../api/index.md)
- [IXCoach API Tests Map](../tests/TEST-MAP.md)

This document provides a comprehensive overview of all intent-documented functions in the ixcoach-api repository, organized by category and status.

## Documentation Status Overview

| Status            | Count | Percentage | Description                                                     |
| ----------------- | ----- | ---------- | --------------------------------------------------------------- |
| 🟢 Complete       | 55    | 38%        | Function documented with "When X, this aims to Y" format        |
| 🟠 P1-Next        | 10    | 7%         | Webhook functions prioritized for immediate documentation       |
| 🟠 P2-Next        | 5     | 3%         | Feature access functions prioritized for next documentation     |
| 🟠 P3-Next        | 5     | 3%         | User account functions prioritized for documentation            |
| 🟠 P4-Next        | 4     | 3%         | Payment functions prioritized for documentation                 |
| 🟠 P5-Next        | 3     | 2%         | Marketing functions prioritized for documentation               |
| 🟡 Backlog        | 63    | 44%        | Functions identified but not yet prioritized for documentation  |

## Function Documentation Status

| Priority | Category | Function | Status | Intent | File Path |
|----------|----------|----------|--------|--------|-----------|
| 1 | Webhook | `handleInvoicePaymentSucceeded` | 🟠 P1-Next | When a payment is successful, this aims to process it for subscription activation | `/subscriptions-and-stripe-webhooks/event-handlers/invoice.payment_succeeded.js` |
| 1 | Webhook | `handleInvoicePaymentFailed` | 🟠 P1-Next | When a payment fails, this aims to manage access while facilitating recovery attempts | `/subscriptions-and-stripe-webhooks/event-handlers/invoice.payment_failed.js` |
| 1 | Webhook | `extractUserIdFromWebhook` | 🟠 P1-Next | When a webhook is received, this aims to reliably identify the affected user with multiple fallback strategies | `/subscriptions-and-stripe-webhooks/entity-resolvers/user-id-extractor.js` |
| 1 | Webhook | `extractProductIdFromWebhook` | 🟠 P1-Next | When processing subscription events, this aims to identify the purchased product for plan assignment | `/subscriptions-and-stripe-webhooks/entity-resolvers/product-id-extractor.js` |
| 1 | Webhook | `handleSubscriptionCreated` | 🟠 P1-Next | When a new subscription is created, this aims to update user plan status across multiple systems | `/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.created.js` |
| 1 | Webhook | `handleSubscriptionDeleted` | 🟠 P1-Next | When a subscription is cancelled, this aims to apply appropriate access changes while respecting "once a customer" policy | `/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.deleted.js` |
| 1 | Webhook | `handleCustomerCreated` | 🟠 P1-Next | When a customer is created in Stripe, this aims to synchronize records across systems | `/subscriptions-and-stripe-webhooks/event-handlers/customer.created.js` |
| 1 | Webhook | `handleSubscriptionUpdated` | 🟠 P1-Next | When a subscription plan changes, this aims to update user access and entitlements accordingly | `/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.updated.js` |
| 1 | Webhook | `validateWebhookData` | 🟠 P1-Next | When receiving webhook data, this aims to verify integrity before processing to prevent errors | `/subscriptions-and-stripe-webhooks/validation/webhook-validator.js` |
| 1 | Webhook | `ensureWebhookIdempotency` | 🟠 P1-Next | When processing webhooks, this aims to prevent duplicate handling of the same event | `/subscriptions-and-stripe-webhooks/utils/idempotency-handler.js` |
| 1 | Authentication | `firebaseRegister` | 🟢 Complete | When a user attempts to create an account via Firebase, this aims to securely register them while preserving activity data | `/controllers/authController.js` |
| 1 | Authentication | `createGuestUser` | 🟢 Complete | When a user wants to try the product without registration, this aims to create a temporary account that enables immediate usage | `/controllers/authController.js` |
| 1 | Authentication | `login` | 🟢 Complete | When a user attempts to sign in, this aims to authenticate them through multiple verification methods with progressive fallbacks | `/controllers/authController.js` |
| 1 | Authentication | `generateOneClickLogin` | 🟢 Complete | When frictionless authentication is needed, this aims to generate secure, time-limited login links that allow passwordless access | `/utils/auth/oneClickEmail.js` |
| 1 | Authentication | `validateEmail` | 🟢 Complete | When users enter email addresses, this aims to provide immediate feedback for invalid formats with specific validation errors | `/account-management/validation/email-validator.js` |
| 1 | Authentication | `normalizeEmail` | 🟢 Complete | When processing user-provided email addresses, this aims to standardize formats for consistent authentication | `/account-management/validation/email-validator.js` |
| 2 | Feature Access | `handleFeatureRequest` | 🟠 P2-Next | When users attempt to access premium features, this aims to serve as the central gateway for access control | `/controllers/featureController.js` |
| 2 | Feature Access | `checkUsageAllowance` | 🟠 P2-Next | When users consume limited resources, this aims to verify remaining quota availability | `/utils/subscriptionUtils.js` |
| 2 | Feature Access | `applyFeatureLimits` | 🟠 P2-Next | When users access tier-specific features, this aims to enforce appropriate usage limits | `/middlewares/featureLimits.js` |
| 2 | Feature Access | `recordFeatureUsage` | 🟠 P2-Next | When users consume quota-limited features, this aims to track usage for billing and limitations | `/utils/usageTracker.js` |
| 2 | Feature Access | `calculateRemainingUsage` | 🟠 P2-Next | When displaying user limits, this aims to accurately show remaining feature allowances | `/utils/usageCalculator.js` |
| 2 | Feature Access | `checkFeatureUsage` | 🟢 Complete | When a user attempts to access premium features, this aims to validate their subscription status | `/utils/subscriptionUtils.js` |
| 2 | Feature Access | `hasAccess` | 🟢 Complete | When determining if a user can access premium functionality, this aims to provide a standardized check | `/utils/subscriptionUtils.js` |
| 2 | Feature Access | `isFreePlanUser` | 🟢 Complete | When determining access restrictions, this aims to distinguish between true free users and trial users | `/utils/subscriptionUtils.js` |
| 2 | Feature Access | `getTrialStatus` | 🟢 Complete | When making access control decisions, this aims to provide accurate trial eligibility and remaining time | `/utils/subscriptionUtils.js` |
| 2 | Subscription | `resolvePlanFromSubscription` | 🟢 Complete | When a Stripe webhook delivers subscription data, this aims to identify the correct internal plan to assign | `/subscriptions-and-stripe-webhooks/entity-resolvers/plan-resolver.js` |
| 2 | Subscription | `ensureTrialUserHasCorrectPlan` | 🟢 Complete | When a user enters a credit card trial, this aims to ensure they receive immediate premium-level access | `/utils/subscriptionUtils.js` |
| 2 | Subscription | `getPlans` | 🟢 Complete | When a user explores subscription options, this aims to present personalized plan offerings | `/controllers/planController.js` |
| 2 | Subscription | `startNewTrial` | 🟢 Complete | When a free user wants to experience premium features, this aims to initiate a time-limited trial period | `/utils/subscriptionUtils.js` |
| 2 | Subscription | `determineAnySubscriptionAccess` | 🟢 Complete | When a user attempts to access premium features, this aims to implement the "once a customer, always a customer" policy | `/subscriptions-and-stripe-webhooks/state-determiners/subscription-access-determiner.js` |
| 2 | Subscription | `resolvePlanByStripePriceId` | 🟢 Complete | When processing subscription purchases, this aims to map Stripe price IDs to internal plan levels | `/subscriptions-and-stripe-webhooks/entity-resolvers/plan-resolver.js` |
| 3 | User Account | `syncUserProfile` | 🟠 P3-Next | When user data changes, this aims to ensure profile consistency across MongoDB and Firebase | `/utils/profileSync.js` |
| 3 | User Account | `mergeUserAccounts` | 🟠 P3-Next | When duplicate accounts are detected, this aims to combine them while preserving all data | `/utils/accountMerger.js` |
| 3 | User Account | `handlePasswordChange` | 🟠 P3-Next | When users update passwords, this aims to securely process changes across all auth systems | `/controllers/authController.js` |
| 3 | User Account | `initiatePasswordReset` | 🟠 P3-Next | When users forget passwords, this aims to start the secure recovery process | `/controllers/authController.js` |
| 3 | User Account | `validateResetToken` | 🟠 P3-Next | When users attempt password reset, this aims to verify security token validity | `/controllers/authController.js` |
| 3 | User Journey | `getUserJorney` | 🟢 Complete | When a new user begins exploring the product, this aims to track their progress through key discovery milestones | `/controllers/userJorneyController.js` |
| 3 | User Journey | `updateUserJorney` | 🟢 Complete | When a user completes a significant milestone, this aims to persistently record their progress | `/controllers/userJorneyController.js` |
| 3 | User Journey | `updatePurhaseIntent` | 🟢 Complete | When a user demonstrates interest in upgrading, this aims to capture these purchase intent signals | `/controllers/userJorneyController.js` |
| 3 | User Journey | `determineUserFunnelStage` | 🟢 Complete | When business decisions require user segmentation, this aims to categorize users into appropriate funnel stages | `/utils/funnelstages/funnelStages.js` |
| 3 | User Journey | `trackTrialInitialized` | 🟢 Complete | When a user enters a credit card trial, this aims to track this critical conversion event for attribution | `/utils/meta/trialInitialized.js` |
| 3 | Engagement | `saveUserPreference` | 🟢 Complete | When a user configures their re-engagement preferences, this aims to personalize their follow-up experience | `/controllers/UserEngagementController.js` |
| 3 | Engagement | `getUserPreference` | 🟢 Complete | When determining how to communicate with users, this aims to retrieve their engagement preferences | `/controllers/UserEngagementController.js` |
| 3 | Engagement | `getAllUserEmailReengagement` | 🟢 Complete | When the application starts or settings change, this aims to establish automated re-engagement schedules | `/controllers/UserEngagementController.js` |
| 3 | Engagement | `turnOffAIMachine` | 🟢 Complete | When a user wants to stop receiving AI communications, this aims to immediately honor their preference | `/controllers/UserEngagementController.js` |
| 3 | Engagement | `updateDefaultEmailEngagement` | 🟢 Complete | When a user's subscription plan changes, this aims to automatically adjust their communication preferences | `/controllers/UserEngagementController.js` |
| 3 | Engagement | `sendReminderEmail` | 🟢 Complete | When a user refers a friend who hasn't completed registration, this aims to send a personalized reminder | `/controllers/emailController.js` |
| 3 | Engagement | `sendDowngradeEmail` | 🟢 Complete | When a user downgrades their subscription, this aims to acknowledge their decision while creating future re-engagement opportunities | `/controllers/emailController.js` |
| 4 | Payment | `createSession` | 🟢 Complete | When a user decides to purchase a subscription, this aims to create a seamless checkout experience | `/controllers/stripeController.js` |
| 4 | Payment | `createCheckoutSession` | 🟠 P4-Next | When initiating a purchase flow, this aims to create a secure Stripe checkout session | `/controllers/stripeController.js` |
| 4 | Payment | `handlePaymentMethodAttached` | 🟠 P4-Next | When users add payment methods, this aims to update their account for immediate use | `/subscriptions-and-stripe-webhooks/event-handlers/payment_method.attached.js` |
| 4 | Payment | `generatePaymentLink` | 🟠 P4-Next | When users need personalized payment options, this aims to create secure individual links | `/utils/paymentLinks.js` |
| 4 | Payment | `processRefund` | 🟠 P4-Next | When refunding customers, this aims to handle the transaction while maintaining appropriate records | `/controllers/refundController.js` |
| 4 | Email | `isMicrosoftEmail` | 🟢 Complete | When sending emails to users, this aims to detect Microsoft email clients that require special formatting | `/utils/email/emailClientDetector.js` |
| 4 | Email | `isValidEmail` | 🟢 Complete | When users provide email addresses, this aims to validate format correctness to prevent authentication errors | `/email/helpers/email-validation.js` |
| 4 | Email | `extractAndAnalyzeLinks` | 🟢 Complete | When preparing email communications, this aims to analyze content links for metrics tracking | `/email/operations/extract-email-links.js` |
| 4 | Email | `formatForMicrosoftClient` | 🟢 Complete | When sending emails to Microsoft client users, this aims to transform content to avoid malware detection | `/email/formatters/microsoft-formatter.js` |
| 4 | Email | `resolveEmailClient` | 🟢 Complete | When preparing to send emails, this aims to determine the recipient's email client type | `/email/entity-resolvers/email-client-resolver.js` |
| 4 | Email | `normalizeHtmlContent` | 🟢 Complete | When processing email templates, this aims to ensure proper HTML structure for consistent rendering | `/email/helpers/email-validation.js` |
| 4 | Email | `replaceLinksWithText` | 🟢 Complete | When sending to email clients that block clickable elements, this aims to preserve message integrity | `/email/helpers/html-manipulation.js` |
| 4 | Email | `addAfterBodyOpen` | 🟢 Complete | When email clients have specific rendering issues, this aims to insert warning banners | `/email/helpers/html-manipulation.js` |
| 4 | Email | `validateTextContent` | 🟢 Complete | When preparing email communications, this aims to ensure non-empty valid string content | `/email/helpers/email-validation.js` |
| 4 | Email | `applyClientSpecificFormat` | 🟢 Complete | When sending emails to different email providers, this aims to adapt email content for client-specific requirements | `/email/operations/apply-client-specific-format.js` |
| 5 | UI | `preserveListPosition` | 🟢 Complete | When users navigate between screens, this aims to maintain their exact position in lists | `/account-management/ui/navigation-handler.js` |
| 5 | UI | `renderConfirmation` | 🟢 Complete | When users need to confirm important actions, this aims to create accessible confirmation dialogs | `/account-management/ui/menu-renderer.js` |
| 5 | UI | `showErrorOverlay` | 🟢 Complete | When errors occur during user operations, this aims to display meaningful error messages without disrupting navigation | `/account-management/ui/navigation-handler.js` |
| 5 | UI | `navigateTo` | 🟢 Complete | When users move between different screens, this aims to preserve their context and state | `/account-management/ui/navigation-handler.js` |
| 5 | UI | `goBack` | 🟢 Complete | When users need to return to previous screens, this aims to restore their exact previous position | `/account-management/ui/navigation-handler.js` |
| 5 | Marketing | `trackConversionEvent` | 🟠 P5-Next | When users complete valuable actions, this aims to record these events for marketing attribution | `/utils/conversionTracking.js` |
| 5 | Marketing | `sendFacebookConversion` | 🟠 P5-Next | When tracking ad performance, this aims to report conversion data to Facebook for ROI calculation | `/utils/meta/facebookEvents.js` |
| 5 | Marketing | `captureAttributionData` | 🟠 P5-Next | When users arrive from marketing campaigns, this aims to preserve source data for analytics | `/utils/attributionCapture.js` |
| 5 | Marketing | `formatFbc` | 🟢 Complete | When tracking ad campaign effectiveness, this aims to properly format Facebook click IDs to ensure accurate attribution | `/utils/meta/facebookEvents.js` |
| 5 | Marketing | `buildUtmLog` | 🟢 Complete | When analyzing marketing effectiveness, this aims to create structured, human-readable representations of UTM parameters | `/utils/meta/facebookEvents.js` |

## Critical Path User Journey Documentation

The following functional pathways represent key user journeys that are now fully documented:

1. **User Registration to Trial Conversion Path**
   - `createGuestUser` → `firebaseRegister` → `startNewTrial` → `trackTrialInitialized` → `ensureTrialUserHasCorrectPlan`
   - This critical path documents how guest users convert to registered users and enter trials

2. **Subscription Access Determination Path**
   - `resolvePlanFromSubscription` → `hasAccess` → `determineAnySubscriptionAccess` → `isFreePlanUser`
   - This pathway implements the core "once a customer, always a customer" business policy

3. **User Engagement Preferences Path**
   - `saveUserPreference` → `getUserPreference` → `getAllUserEmailReengagement` → `updateDefaultEmailEngagement`
   - This pathway documents how user communication preferences are managed across their lifecycle

4. **Email Client Optimization Path**
   - `resolveEmailClient` → `isMicrosoftEmail` → `formatForMicrosoftClient` → `replaceLinksWithText` → `normalizeHtmlContent`
   - This pathway ensures email deliverability across different email clients

## Next 50 Functions for Documentation (Prioritized)

The following 50 functions are prioritized for documentation based on their business impact and role in critical user journeys:

### Webhook Handling (Highest Priority)

1. **`handleInvoicePaymentSucceeded`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/invoice.payment_succeeded.js`
   - Core function for processing successful payments and activating subscriptions

2. **`handleInvoicePaymentFailed`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/invoice.payment_failed.js`
   - Critical for managing failed payments and maintaining appropriate user access

3. **`extractUserIdFromWebhook`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/entity-resolvers/user-id-extractor.js`
   - Resolves user identity from webhook data with multiple fallback strategies

4. **`extractProductIdFromWebhook`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/entity-resolvers/product-id-extractor.js`
   - Identifies purchased products from webhook data for plan assignment

5. **`handleSubscriptionCreated`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.created.js`
   - Processes new subscription creation events for immediate access provisioning

6. **`handleSubscriptionDeleted`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.deleted.js`
   - Manages subscription cancellations while preserving appropriate access

7. **`handleCustomerCreated`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.created.js`
   - Processes new customer creation in Stripe for user record synchronization

8. **`handleSubscriptionUpdated`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.updated.js`
   - Manages subscription changes including upgrades and downgrades

9. **`validateWebhookData`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/validation/webhook-validator.js`
   - Ensures webhook data integrity before processing to prevent errors

10. **`ensureWebhookIdempotency`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/utils/idempotency-handler.js`
    - Prevents duplicate processing of the same webhook events

### Feature Access Control (High Priority)

11. **`handleFeatureRequest`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/featureController.js`
    - Central gateway for controlling access to premium features

12. **`checkUsageAllowance`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/subscriptionUtils.js`
    - Determines if a user has remaining usage quota for specific features

13. **`applyFeatureLimits`** - `/Users/jedi/react_projects/ix/ixcoach-api/middlewares/featureLimits.js`
    - Enforces tier-specific usage limits across the platform

14. **`recordFeatureUsage`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/usageTracker.js`
    - Tracks consumption of limited resources for quota management

15. **`calculateRemainingUsage`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/usageCalculator.js`
    - Determines remaining allowance of limited features for UI display

16. **`checkPremiumAccessEligibility`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/accessEligibility.js`
    - Evaluates if a user qualifies for premium features through various means

17. **`enforceUsageLimits`** - `/Users/jedi/react_projects/ix/ixcoach-api/middlewares/usageLimits.js`
    - Applies rate limiting and quota restrictions based on subscription tier

18. **`getFeatureFlagStatus`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/featureFlags.js`
    - Determines if specific features are enabled for a user based on various criteria

19. **`checkTrialFeatureAccess`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/trialAccess.js`
    - Specialized access control for users in trial periods

20. **`validateAccessToken`** - `/Users/jedi/react_projects/ix/ixcoach-api/middlewares/tokenValidator.js`
    - Verifies authentication tokens for secure feature access

### User Account Management (High Priority)

21. **`syncUserProfile`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/profileSync.js`
    - Ensures user profile consistency across MongoDB and Firebase

22. **`mergeUserAccounts`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/accountMerger.js`
    - Combines multiple user accounts when duplicates are detected

23. **`handlePasswordChange`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Manages secure password updates across authentication systems

24. **`initiatePasswordReset`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Starts the password recovery process for users who forgot their password

25. **`validateResetToken`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Verifies password reset tokens for security during password recovery

26. **`completePasswordReset`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Finalizes password reset process with new credentials

27. **`linkAuthProvider`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Connects additional authentication methods to existing accounts

28. **`removeAuthProvider`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Disconnects authentication methods while maintaining account access

29. **`updateUserEmail`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/authController.js`
    - Handles email changes with verification and system-wide updates

30. **`getUserProfile`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/userController.js`
    - Retrieves comprehensive user information for profile display

### Payment Processing (Medium Priority)

31. **`createCheckoutSession`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/stripeController.js`
    - Initiates the Stripe checkout process for new subscriptions

32. **`handlePaymentMethodAttached`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/payment_method.attached.js`
    - Processes new payment method events for user accounts

33. **`generatePaymentLink`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/paymentLinks.js`
    - Creates personalized payment links for individual users

34. **`processRefund`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/refundController.js`
    - Handles customer refund requests with appropriate account updates

35. **`handleCustomerSubscriptionPaused`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.paused.js`
    - Processes subscription pause events while maintaining appropriate access

36. **`handleCustomerSubscriptionResumed`** - `/Users/jedi/react_projects/ix/ixcoach-api/subscriptions-and-stripe-webhooks/event-handlers/customer.subscription.resumed.js`
    - Processes subscription resumption after pauses

37. **`createBillingPortalSession`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/stripeController.js`
    - Generates secure links to Stripe's billing portal for subscription management

38. **`updatePaymentMethod`** - `/Users/jedi/react_projects/ix/ixcoach-api/controllers/stripeController.js`
    - Handles payment method updates for existing subscriptions

39. **`recordPaymentFailure`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/paymentTracking.js`
    - Logs payment failures for analytics and recovery attempts

40. **`initiateRecoveryFlow`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/paymentRecovery.js`
    - Starts the process to recover failed payments through user outreach

### Marketing & Analytics (Medium Priority)

41. **`trackConversionEvent`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/conversionTracking.js`
    - Records key conversion events for marketing attribution

42. **`sendFacebookConversion`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/meta/facebookEvents.js`
    - Reports conversion data to Facebook for ad optimization

43. **`captureAttributionData`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/attributionCapture.js`
    - Records marketing source information from user acquisition channels

44. **`trackUserActivity`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/activityTracking.js`
    - Monitors user engagement for retention analysis

45. **`generateCampaignUrl`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/campaignUrlBuilder.js`
    - Creates tracked URLs for marketing campaigns with attribution parameters

46. **`recordCampaignImpression`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/campaignTracking.js`
    - Logs when users view marketing campaigns for attribution

47. **`captureReferralSource`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/referralTracking.js`
    - Records information about how users were referred to the platform

48. **`trackSessionStart`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/sessionTracking.js`
    - Monitors user session beginnings for engagement metrics

49. **`trackFeatureDiscovery`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/discoveryTracking.js`
    - Records when users discover and engage with specific features

50. **`generateActivityReport`** - `/Users/jedi/react_projects/ix/ixcoach-api/utils/reportGenerator.js`
    - Creates structured reports on user activity for business analysis

## Documentation Progress Summary

Based on the function distribution, these areas are prioritized for documentation:

1. **Webhook Handling Functions (0/25 Complete)**
   - Functions that process Stripe webhook events are critical for payment processing
   - Need to document the specific intent of different webhook handler functions

2. **User Account Management Functions (2/30 Complete)**
   - Functions that manage user account data synchronization between Firebase and MongoDB
   - Need to document user profile management functions beyond basic authentication

3. **Feature Access Control Functions (5/20 Complete)**
   - Complete documentation of the feature access control system
   - Need to document functions that implement tier-specific feature limitations

4. **Payment Processing Functions (4/25 Complete)**
   - Documentation of payment handling beyond subscription management
   - Need to document payment method storage and recovery workflows

5. **Marketing Attribution Functions (3/15 Complete)**
   - Functions that process marketing attribution data for ROI calculations
   - Need to document functions that connect with external marketing platforms