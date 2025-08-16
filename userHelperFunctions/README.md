# User Helper Functions

This directory contains atomic functions for user analysis, specifically focused on funnel stage determination and user status evaluation.

## Overview

These functions are designed to determine a user's position in the conversion funnel based on their account status, subscription data, and activity patterns. The system follows a consistent architecture with the following components:

1. **Atomic Functions**: Single-purpose functions, each in their own file for clear maintainability
2. **Registry**: A central registry that imports and exports all functions
3. **Resolvers**: Functions that make determinations about user state 
4. **Validators**: Functions that validate specific conditions

## File Structure

```
ixcoach-api/
├── userHelperFunctions/
│   ├── activity/          # User activity analysis functions
│   ├── subscription/      # Subscription-related helper functions 
│   ├── errors/            # Error detection functions
│   ├── tracking/          # User tracking and analytics functions
│   ├── engagement/        # User engagement analysis
│   ├── formatValue.js     # Formats values for display
│   ├── generateWhereValues.js # Generates MongoDB queries for stages
│   ├── getFieldMeaning.js # Explains field meanings
│   ├── getNestedValue.js  # Gets nested values from objects
│   ├── getStageId.js      # Generates consistent IDs for stages
│   ├── getStageLogic.js   # Explains logic for funnel stages
│   ├── getStageRecommendation.js # Provides recommendations for stages
│   ├── getStageRelevantFields.js # Lists fields relevant to stages
│   ├── index.js           # Main entry point/exports
│   └── registry.js        # Central registry of all functions
├── resolvers/
│   ├── hasInitiatedCheckout.js   # Determines if checkout started
│   ├── isActiveSubscriber.js     # Checks if user is active subscriber
│   ├── isUserAtRisk.js           # Determines at-risk users
│   ├── isUserInChurningState.js  # Checks for cancellation intent
│   ├── isUserOnCCTrial.js        # Checks for CC trial status
│   ├── isUserOnLegacyTrial.js    # Checks for legacy trial status
│   ├── isUserRegistered.js       # Verifies if user is registered
│   └── resolveUserFunnelStage.js # Main funnel stage resolver
├── validators/
│   ├── getUserTrialStatus.js     # Gets detailed trial status
│   ├── hasUserAbandonedCheckout.js # Checks for abandoned checkout
│   ├── isUserActive.js           # Validates user active status
│   └── isUserInChurnedState.js   # Validates churned state
├── config/
│   └── funnelStagesConstants.js  # Funnel stage constants and metadata
└── utils/
    └── dateUtils.js              # Date handling utilities
```

## Usage

To use these functions in your code:

```javascript
// Import the registry for access to all functions
const userHelpers = require('../userHelperFunctions');

// Or import specific functions directly
const { resolveUserFunnelStage } = require('../userHelperFunctions');
const getStageId = require('../userHelperFunctions/getStageId');

// Example usage
const user = { 
  email: 'user@example.com',
  lastLogin: new Date('2025-04-01'),
  currentPlan: 2
};

const stripeData = {
  stripeSubscriptionData: [{ 
    status: 'active',
    cancel_at_period_end: false,
    plan: { amount: 2500 }
  }],
  stripeCustomerData: { id: 'cus_123456' }
};

// Determine user's funnel stage
const funnelStage = resolveUserFunnelStage(user, stripeData);
console.log(`User is in stage: ${funnelStage}`);

// Get additional information about the stage
const { getFunnelStages } = require('../config/funnelStagesConstants');
const stageInfo = getFunnelStages(funnelStage).find(stage => stage.id === funnelStage);
console.log(`Stage description: ${stageInfo.description}`);
```

## Architecture

This system follows a modular architecture that separates concerns:

1. **Registry Pattern**: Central registry imports and exports all functions
2. **Single Responsibility**: Each file contains one function
3. **Function Categories**: Functions are organized by category in subfolders
4. **Hierarchical Resolution**: Funnel stages are evaluated in order of precedence

## Maintenance

When adding new functions:

1. Create a new file in the appropriate directory
2. Implement the function with clear documentation
3. Add imports and exports to the registry.js file
4. Update the index.js file to expose the function

## Consistent Naming

Functions follow consistent naming patterns:

- `is*` functions return boolean values (e.g., isUserRegistered)
- `has*` functions check for the existence of properties/conditions
- `get*` functions retrieve or calculate values
- `generate*` functions create new data structures

## Business Logic

The funnel stage resolution follows this hierarchical logic:

1. Check if user has churned (canceled subscription)
2. Check if user is in churning state (set to cancel)
3. Check if user is at risk (inactive with subscription)
4. Check if user is an active subscriber
5. Check if user is on a CC trial
6. Check if user is on a legacy trial
7. Check if user has abandoned checkout
8. Check if user has initiated checkout
9. Check if user is registered
10. Default to 'new' if no other conditions are met