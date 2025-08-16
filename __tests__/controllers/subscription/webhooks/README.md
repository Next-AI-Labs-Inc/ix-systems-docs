# Stripe Webhook Test Suite

This directory contains a comprehensive test suite for the Stripe webhook handlers in the IXCoach API. These tests ensure proper subscription lifecycle management based on webhook events from Stripe.

## Test Files

All test files are prefixed with `BUILDING:` in their describe blocks to indicate they are in development. When completed and passing, they should be updated to have a 🔥 emoji prefix.

### Core Webhook Tests

1. **checkout-session-completed.test.js**
   - Tests initial subscription creation after checkout
   - Verifies trial period handling
   - Checks user plan updates

2. **invoice-payment-succeeded.test.js**
   - Tests payment tracking and subscription status updates
   - Verifies Meta/Facebook conversion events
   - Handles $0 trial payments properly

3. **customer-subscription-deleted.test.js**
   - Tests subscription cancellation and user downgrade
   - Verifies cancellation emails
   - Preserves subscription history

4. **customer-subscription-updated.test.js**
   - Tests plan changes and status updates
   - Handles scheduled cancellations
   - Maintains subscription history

5. **customer-subscription-created.test.js**
   - Tests subscription creation with and without trials
   - Verifies welcome emails
   - Ensures immediate access activation

### Infrastructure Tests

6. **webhook-idempotency.test.js**
   - Tests duplicate webhook handling
   - Verifies proper response for repeated events
   - Ensures event handlers are only called once

## Mock Data

All tests use the CONFIRMED mocks from `/subscriptions-and-stripe-webhooks/mocks/` directory when available, or properly documented fallback mocks where necessary.

## UX Impact

Each test is focused on a specific user experience aspect:

- **Immediate access** after payment or signup
- **Proper downgrade** when subscriptions end
- **Accurate tracking** of conversions for marketing
- **Trial period handling** for seamless onboarding
- **Safe cancellation** with clear communication

## Running Tests

To run all webhook tests:

```bash
jest -t "BUILDING: .* Webhook" --config=jest.config.js
```

To run a specific test:

```bash
jest -t "BUILDING: Checkout Session Completed Webhook Handler" --config=jest.config.js
```

## Implementing New Tests

When implementing additional tests:

1. Use official Stripe mock data whenever possible
2. Document UX impact for each test scenario
3. Include both happy path and error handling tests
4. Maintain idempotency in all webhook tests

## Status Progression

Tests should progress through these statuses in their describe blocks:

1. `BUILDING:` - Test is being created
2. `TESTING:` - Test is complete but not passing
3. `WORKING:` - Test is passing but not reviewed
4. `🔥` - Test is complete, passing, and human-verified