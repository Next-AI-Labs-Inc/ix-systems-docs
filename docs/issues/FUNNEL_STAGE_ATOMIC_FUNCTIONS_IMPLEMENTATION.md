# Funnel Stage Atomic Functions Migration from admin4

## What Was Requested

Clone the funnel stage atomic functions from the admin4 repository to the ixcoach-api repository:
- ✅ Preserve exact business logic 
- ✅ Use same folder structure where appropriate
- ✅ One function per file with file name matching function
- ✅ Handle imports/exports systematically to avoid broken references

## Implementation Structure

The implementation uses a three-tier architecture:

1. **Atomic Validators** (`/validators/*.js`) - Each validation function in its own file
2. **Atomic Resolvers** (`/resolvers/*.js`) - Each resolver function in its own file
3. **Helper Functions** (`/userHelperFunctions/*.js`) - Higher-level functions that use the validators and resolvers

## Implementation Status

### Core Validator Functions
- 🟢 `isUserInChurnedState.js` - Completed and tested
- 🟢 `isUserActive.js` - Completed and tested
- 🟢 `hasUserAbandonedCheckout.js` - Completed and tested
- 🟢 `getUserTrialStatus.js` - Completed and tested

### Core Resolver Functions
- 🟢 `resolveUserFunnelStage.js` - Completed and tested
- 🟢 `isUserInChurningState.js` - Completed and tested
- 🟢 `isUserAtRisk.js` - Completed and tested
- 🟢 `isActiveSubscriber.js` - Completed and tested
- 🟢 `isUserOnCCTrial.js` - Completed and tested
- 🟢 `isUserOnLegacyTrial.js` - Completed and tested
- 🟢 `hasInitiatedCheckout.js` - Completed and tested
- 🟢 `isUserRegistered.js` - Completed and tested

### Helper Functions
- 🟢 `getStageId.js` - Completed and tested
- 🟢 `getStageLogic.js` - Completed and tested
- 🟢 `getStageRelevantFields.js` - Completed and tested
- 🟢 `getStageRecommendation.js` - Completed and tested
- 🟢 `getNestedValue.js` - Completed and tested
- 🟢 `formatValue.js` - Completed and tested
- 🟢 `getFieldMeaning.js` - Completed and tested
- 🟢 `generateWhereValues.js` - Completed and tested

### Registry and Exports
- 🟢 `registry.js` - Completed with all imports
- 🟢 `index.js` - Completed with all exports

### Shared Utilities & Constants
- 🟢 Updated `utils/dateUtils.js` - Added required date utility functions
- 🟢 Created `config/funnelStagesConstants.js` - Contains stage definitions and metadata

## Using The System

```javascript
// Import the registry for all functions
const funnelHelpers = require('../userHelperFunctions');

// Or import individual functions
const resolveUserFunnelStage = require('../resolvers/resolveUserFunnelStage');
const isUserAtRisk = require('../resolvers/isUserAtRisk');

// Determine user's funnel stage
const stage = resolveUserFunnelStage(user, { 
  stripeSubscriptionData,
  stripeCustomerData 
});
```

## Migration Notes

- Used existing `dateUtils.js` rather than creating a duplicate
- Maintained CommonJS module format for compatibility
- All original business logic remains exactly the same
- Each function is in its own file for cleaner maintenance