# IXCoach API Registry

This document provides a comprehensive inventory of all exported functions in the ixcoach-api repository, organized by directory.

## Table of Contents

- [Controllers](#controllers)
- [Utils](#utils)
- [Middlewares](#middlewares)
- [Models](#models)
- [Resolvers](#resolvers)
- [User Helper Functions](#user-helper-functions)
- [Validators](#validators)
- [Routes](#routes)

## Controllers

The controllers directory contains all API endpoint handlers and business logic.

### authController.js

- `firebaseRegister` - Registers a user using Firebase authentication
- `createGuestUser` - Creates a guest user account
- `register` - Traditional email/password registration
- `login` - Authenticates a user
- `startTrial` - Initiates a free trial
- `getProfile` - Retrieves authenticated user's profile
- `updatePassword` - Updates a user's password
- `getSubscriptionDetails` - Gets details about a user's subscription
- `changeUserStatus` - Changes a user status property
- `isPhoneNumberRegistered` - Checks if a phone number is registered
- `checkEmailStatus` - Checks email authentication status
- `addAuthentication` - Adds authentication to existing accounts
- `enablePasswordProvider` - Enables password provider for users
- `checkPasswordProvider` - Checks if a user has password provider
- `generateCannyToken` - Generates an SSO token for Canny
- `checkTrial` - Verifies trial eligibility and status
- `finduserAccountWithDetails` - Finds user accounts with details

### CustomAIController.js

- `createCustomAI_or_update` - Creates or updates a custom AI
- `deleteCustomAI` - Deletes a custom AI
- `getUserCustomAIs` - Gets custom AIs for a user
- `getCustomAiById` - Gets a specific custom AI
- `uploadAvatar` - Uploads an avatar for a custom AI
- `generate` - Generates content with a custom AI
- `getAllPublicCustomAIs` - Gets all public custom AIs
- `getAllReferralCodes` - Gets all referral codes
- `createCategoriesTags` - Creates categories and tags
- `addTag` - Adds a tag to a custom AI
- `updateTag` - Updates a tag
- `getTags` - Gets all tags
- `getTagWiseAIs` - Gets AIs by tag
- `updatePublishOptions` - Updates publishing options
- `updateApprovalStatus` - Updates approval status
- `generateProfilePhoto` - Generates a profile photo
- `generateLink` - Generates a sharing link
- `getCustomAiMethodByTitle` - Gets a method by title
- `getLatestCode` - Gets the latest code
- `InviteEmail` - Sends an invitation email
- `createConversation` - Creates a conversation
- `uploadKnowledgeFiles` - Uploads knowledge files

### CoachingSessionController.js

- `getSessions` - Gets all coaching sessions
- `getCurrentSystemPrompt` - Gets the current system prompt
- `getCurrentUserStep` - Gets the current user step
- `getConversations` - Gets all conversations
- `CreatePracticeSession` - Creates a practice session
- `addMessage` - Adds a message to a session
- `regeneratePrompt` - Regenerates the prompt
- `editAiResponse` - Edits an AI response
- `updateConversationTitle` - Updates a conversation title
- `updateConversationMethod` - Updates a conversation method
- `updatePromptAndRegenerate` - Updates prompt and regenerates
- `deleteConversation` - Deletes a conversation
- `getSessionsByGoal` - Gets sessions by goal
- `logSessionInteraction` - Logs a session interaction

### ObjectiveController.js

- `createObjective` - Creates an objective
- `getUserNameAndIds` - Gets user names and IDs
- `updateObjective` - Updates an objective
- `deleteObjective` - Deletes an objective
- `changeObjectiveStatus` - Changes an objective's status
- `addComment` - Adds a comment to an objective

### SettingController.js

- `createSettings` - Creates settings
- `getSettings` - Gets all settings
- `getSettingsByName` - Gets settings by name
- `updateSettings` - Updates settings
- `updateSettingsV1` - Updates settings (v1)
- `updateByName` - Updates settings by name
- `deleteSettings` - Deletes settings
- `logBots` - Logs bots

### UserEngagementController.js

- `getUserPreference` - Gets user preferences
- `saveUserPreference` - Saves user preferences
- `getAllUserEmailReengagement` - Gets all user email reengagement
- `saveValuesGenerated` - Saves generated values
- `sendTestEmail` - Sends a test email
- `turnOffAIMachine` - Turns off AI machine
- `getRandomUser` - Gets a random user
- `updateDefaultEmailEngagement` - Updates default email engagement

### Other Controllers

- *Visit the full registry for more controllers and their functions*

## Utils

The utils directory contains utility functions and helpers.

### authUtils.js

- `caseInsensitiveEmailSearch` - Case-insensitive email search

### logger.js

- Provides logging functionality with different levels

### subscriptionUtils.js

- `checkYearlyPlanDate` - Checks yearly plan date
- `checkFeatureUsage` - Checks feature usage
- `recordFeatureUsage` - Records feature usage
- `getCurrentPlan` - Gets current plan
- `cancelSubscription` - Cancels subscription
- `resetFeatureUsageIfNecessary` - Resets feature usage if necessary
- `calculateAICreditUsage` - Calculates AI credit usage
- `isLatestTrialExpired` - Checks if latest trial is expired
- `getTrialStatus` - Gets trial status
- `checkAiCreditDollarLimit` - Checks AI credit dollar limit
- `startNewTrial` - Starts a new trial
- `startNewTrialv1` - Starts a new trial (v1)
- `isFreePlanUser` - Checks if user is on free plan
- `hasAccess` - Checks if user has access
- `ensureTrialUserHasCorrectPlan` - Ensures trial user has correct plan
- `ensureAnySubscriptionHasAccess` - Ensures any subscription has access

### userCreationUtility.js

- `createUserRobustly` - Creates a user robustly
- `ensureUserExists` - Ensures a user exists

### promptUtils.js

- `generateExactPrompt` - Generates exact prompt
- `getStyleVariant` - Gets style variant
- `handleAiResponseWithQuotes` - Handles AI response with quotes
- `handleWhereUserIsAtInTheWorkMethod` - Handles where user is at
- `getBkPrompt` - Gets BK prompt
- `customAISystemPrompt` - Custom AI system prompt
- `generateValuesForAI` - Generates values for AI
- *...and many more prompt utilities*

### Other Utils

- *Visit the full registry for more utility functions*

## Middlewares

The middlewares directory contains Express middleware functions.

### auth.js

- Authentication middleware
- `invalidateSubscriptionCache` - Invalidates subscription cache

### authCleaner.js

- Authentication cleanup middleware

### adminAuth.js

- Admin authentication middleware

### teamAuth.js

- Team authentication middleware

### adviserAuth.js

- Adviser authentication middleware

### checkUsage.js

- Usage checking middleware

### errorHandler.js

- Error handling middleware

### validationErrorHandler.js

- Validation error handling middleware

## Models

The models directory contains MongoDB schema definitions.

### User.js

- User model with authentication methods

### Subscription.js

- Subscription model for managing user subscriptions

### Plan.js

- Plan model for subscription plans

### Conversation.js

- Conversation model for AI conversations

### CustomAI.js

- Custom AI model for AI personalization

*...and many more models*

## Resolvers

The resolvers directory contains functions that resolve specific data requirements.

### isActiveSubscriber.js

- Determines if a user is an active subscriber

### isUserInChurningState.js

- Determines if a user is in churning state

### isUserAtRisk.js

- Determines if a user is at risk of churning

### isUserOnCCTrial.js

- Determines if a user is on a credit card trial

### isUserOnLegacyTrial.js

- Determines if a user is on a legacy trial

### hasInitiatedCheckout.js

- Determines if a user has initiated checkout

### isUserRegistered.js

- Determines if a user is registered

### resolveUserFunnelStage.js

- Resolves a user's funnel stage

## User Helper Functions

The userHelperFunctions directory contains functions for user data manipulation.

### formatValue.js

- Formats a value for display based on type and field name

### getNestedValue.js

- Gets a nested value from an object

### getStageId.js

- Gets a stage ID for funnel stages

### getStageLogic.js

- Gets stage logic for funnel stages

### getStageRecommendation.js

- Gets stage recommendation for funnel stages

### getStageRelevantFields.js

- Gets relevant fields for a stage

### index.js (Re-exports)

- Re-exports all user helper functions

### useFunnelStageDeterminer.js

- `useFunnelStageDeterminer` - Determines funnel stage
- `getDetailedFunnelStage` - Gets detailed funnel stage
- `isUserInFunnelStage` - Checks if user is in a funnel stage
- `isUserInFreeStage` - Checks if user is in free stage
- `isUserPaying` - Checks if user is paying
- `isUserInTrial` - Checks if user is in trial

## Validators

The validators directory contains validation functions.

### funnelStageValidators.js

- `isUserInChurnedState` - Validates if a user is in churned state
- `isUserActive` - Validates if a user is active
- `hasUserAbandonedCheckout` - Validates if a user abandoned checkout
- `getUserTrialStatus` - Validates user trial status

### isUserActive.js

- Validates if a user is active

### isUserInChurnedState.js

- Validates if a user is in churned state

### hasUserAbandonedCheckout.js

- Validates if a user has abandoned checkout

### getUserTrialStatus.js

- Gets a user's trial status

## Routes

The routes directory contains Express router definitions.

### auth.js

- Authentication routes

### conversations.js

- Conversation management routes

### subscription.js

- Subscription management routes

### customAI.js

- Custom AI routes

### feedback.js

- Feedback management routes

### webhook.js

- Webhook handling routes

*...and many more route definitions*