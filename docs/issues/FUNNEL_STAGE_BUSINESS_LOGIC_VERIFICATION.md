# Funnel Stage Business Logic Verification

## Business Logic Integrity

✅ **Exact Business Logic Preserved**
- All determinations follow identical logic from admin4
- No modifications to business rules or thresholds
- Identical formulas with the same order of operations

✅ **Trusted Data Sources Respected**
- Uses Stripe data as source of truth for all subscription-related determinations
- Only falls back to MongoDB user data when appropriate (e.g., user.trial flag for legacy trials)
- Explicitly separates data sources in function parameters

✅ **Hierarchy & Precedence Rules Maintained**
- Same stage evaluation order in resolveUserFunnelStage.js
- Same precedence rules (e.g., checking abandoned-checkout before checkout-initiated)
- Same edge case handling (e.g., users with conversationCount = 0)

## Example Business Logic Verification

```javascript
// Original admin4 logic for isUserOnCCTrial
const isUserOnCCTrial = (user, stripeSubscriptionData) => {
  return stripeSubscriptionData.some(sub => {
    const hasPaymentMethod = !!sub.default_payment_method;
    const hasTrialDates = sub.trial_start && sub.trial_end;
    const isPaidPlan = sub.plan && typeof sub.plan.amount === 'number' && sub.plan.amount > 0;
    return hasPaymentMethod && hasTrialDates && isPaidPlan;
  });
};

// Migrated logic - IDENTICAL
const isUserOnCCTrial = (user, stripeSubscriptionData) => {
  return stripeSubscriptionData.some(sub => {
    const hasPaymentMethod = !!sub.default_payment_method;
    const hasTrialDates = sub.trial_start && sub.trial_end;
    const isPaidPlan = sub.plan && typeof sub.plan.amount === 'number' && sub.plan.amount > 0;
    return hasPaymentMethod && hasTrialDates && isPaidPlan;
  });
};
```

## Usage System

### Current Implementation (Required)

The current implementation requires passing in required data every time:

```javascript
const resolveUserFunnelStage = require('../resolvers/resolveUserFunnelStage');

// Get the user's funnel stage
const funnelStage = resolveUserFunnelStage(user, {
  stripeSubscriptionData: user.stripe?.subscriptions?.data || [],
  stripeCustomerData: user.stripe?.customer || null
});
```

### Recommended Wrapper (Not Yet Implemented)

❌ **Wrapper Not Yet Implemented**

We should create a useFunnelStageDeterminer.js file that handles data preparation:

```javascript
// TODO: Create this wrapper file
const useFunnelStageDeterminer = (user) => {
  // Extract Stripe data from user object in the same way admin4 does
  const stripeSubscriptionData = user.stripe?.subscriptions?.data || [];
  const stripeCustomerData = user.stripe?.customer || null;
  
  // Call the resolver with prepared data
  const resolveUserFunnelStage = require('../resolvers/resolveUserFunnelStage');
  return resolveUserFunnelStage(user, {
    stripeSubscriptionData,
    stripeCustomerData
  });
};

module.exports = useFunnelStageDeterminer;
```

## Implementation Tasks

1. 🟢 Migrate all atomic functions (COMPLETE)
2. 🟢 Verify business logic integrity (COMPLETE)
3. 🟢 Create registry and exports (COMPLETE)
4. ⚫ Create useFunnelStageDeterminer wrapper (TODO)
5. ⚫ Add integration tests (TODO)
6. ⚫ Update any relevant API controllers to use the new system (TODO)

## Note on State Management

The admin4 repository pulls Stripe data from the user object, where it has already been attached. To maintain the same pattern, we need to ensure that:

1. The user object has Stripe data attached in the same structure
2. Or we create a wrapper that handles fetching the data in the same way

The recommended approach is to implement the useFunnelStageDeterminer wrapper to maintain consistency with the admin4 implementation.