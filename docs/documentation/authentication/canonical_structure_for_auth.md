# 2025-04-16 Canonical Structure for Authentication and Plan Resolution

## Overview

This document outlines the canonical structure implemented for ensuring consistent subscription plan resolution across the IX system, addressing the issue where users with credit cards in trial periods were intermittently losing their premium access during login/logout cycles.

## Problem Statement

Users who register with email, add a credit card during checkout, and complete registration were experiencing inconsistent plan access. After login/logout cycles, they would temporarily see themselves as free users before the system corrected their plan level.

The root cause was a violation of the Single Source of Truth pattern, where multiple code paths determined user plan levels independently instead of using a central resolver.

## Canonical Structure Implementation

### 1. Central Source of Truth

The system uses a central function as the Single Source of Truth for plan determination:

```
/subscriptions-and-stripe-webhooks/resolvers/determine-effective-subscription-plan.js
```

This function:
- Implements proper caching for efficient Stripe API calls
- Ensures plans in Stripe are synchronized with our database
- Handles users with multiple customer accounts using the same email
- Provides a consistent determination process for all code paths

### 2. Migration Adapter Implementation

Rather than directly modifying existing code, a migration adapter was implemented:

```
/subscriptions-and-stripe-webhooks/migrations/auth-middleware-adapter.js
```

This adapter:
- Redirects all auth middleware plan checks through the central resolver function
- Maintains compatibility with existing code structure
- Preserves logging and debugging capabilities
- Follows the layered architecture pattern for clean separation of concerns

### 3. Cache Invalidation During Logout

A critical aspect of the solution is proper cache invalidation during logout:

```
/routes/auth.js - `/logout` endpoint
```

This endpoint:
- Invalidates both the central plan resolver cache and auth middleware cache
- Ensures fresh plan determination on next login
- Provides detailed invalidation reporting for debugging
- Maintains a record of cache invalidations for auditing

### 4. Frontend Integration

The frontend properly calls the backend logout endpoint:

```
/gptcoach2/src/contexts/AuthContext.js - `logout()` method
```

This method:
- Calls the backend logout API before clearing frontend state
- Handles errors gracefully if the backend call fails
- Cleans up all frontend authentication artifacts
- Maintains backward compatibility with existing flows

## How to Consume This Pattern

When needing to determine a user's subscription plan:

1. **Always use the canonical function:**

```javascript
const { determineEffectiveSubscriptionPlan } = require('../subscriptions-and-stripe-webhooks/resolvers/determine-effective-subscription-plan');

// Then use it to determine the plan
const updatedUser = await determineEffectiveSubscriptionPlan(userId);
if (updatedUser) {
  // User was updated with the correct plan
  user = updatedUser;
}
```

2. **Never call the implementation functions directly:**

```javascript
// DON'T do this:
const trialResult = await ensureTrialUserHasCorrectPlan(userId);

// Instead, use the canonical function or an appropriate adapter
```

3. **For existing code that needs to be migrated:**

Create a migration adapter similar to `auth-middleware-adapter.js` that:
- Imports and calls the canonical function
- Maintains the same function signature as the original
- Provides appropriate logging and error handling
- Documents the migration in comments

4. **When adding new code:**

- Directly use the canonical function
- Document the dependency on the canonical function
- Ensure cache invalidation is handled appropriately
- Follow the layered architecture pattern

## Verification Steps

To verify the fix is working correctly:

1. Register a new user with email and credit card
2. Complete registration on IXC
3. Confirm user sees premium features
4. Log out and log back in
5. Verify user immediately sees premium features (no free tier UI)
6. Check server logs for `[AUTH ADAPTER]` and `[LOGOUT]` messages confirming cache operations

## Technical Details

### Layered Architecture

The solution follows the established layered architecture pattern:

- **Migration Layer:** Provides adapters for existing code to use the new system
- **Resolver Layer:** The canonical function as the central source of truth
- **Handler Layer:** Orchestrates the validation, resolution, determination, and persistence
- **Entity Resolution Layer:** Retrieves entities from databases and APIs
- **State Determination Layer:** Pure functions for determining required changes
- **Persistence Layer:** Handles all database operations
- **Cache Invalidation:** Ensures consistent state across login/logout cycles

### Cache Design

Two separate caches are maintained:

1. **Central Resolver Cache:**
   - In `determine-effective-subscription-plan.js`
   - Used by all direct callers of the canonical function
   - Properly synchronized with Stripe data

2. **Auth Middleware Cache:**
   - In `auth.js`
   - Used by the authentication middleware to minimize API calls
   - Now integrated with the central resolver via the adapter

Both caches are invalidated on logout to ensure fresh state on next login.

## Conclusion

This implementation ensures consistent plan resolution across all code paths while maintaining backward compatibility with existing code. The migration adapter approach allows for gradual adoption of the canonical pattern without requiring a complete rewrite of all authentication and subscription code.