# 2025_05_22_FunnelStageQueue

## Intent

The intent of this document is to describe the asynchronous user funnel stage determination system that processes updates to user marketing and analytics categories when user state changes occur. This system aims to accurately classify users into appropriate marketing funnel stages without blocking the main application flow.

## System Components

### Primary Queue Implementation

The funnelStageQueue is a Bull queue that asynchronously processes user funnel stage determinations to avoid blocking the main application flow. It connects to Redis using environment-specific configuration and maintains isolation between environments through the queue naming pattern.

Location: `/queues/funnelStageQueue.js`

## Functional Flow

1. Queue receives a user object via the job data
2. The process handler calls `determineUserFunnelStage` for assessment
3. The determination function evaluates user against multiple resolvers
4. If a stage change is detected:
   - The user document is updated
   - The funnel stage change timestamp is recorded
   - Special handling occurs for the "churned" state
   - External marketing systems are notified (SendGrid integration)

## Business Logic

The system implements a hierarchical evaluation of user state following a strict priority order:

- [ ] Highest priority: Churned status (subscription fully canceled)
- [ ] High priority: Churning status (cancellation in progress)
- [ ] Medium priority: At-risk status (inactivity with active subscription)
- [ ] Medium priority: Trial status (on credit card trial)
- [ ] Medium priority: Active status (paid subscription)
- [ ] Medium priority: Legacy trial status (old trial system)
- [ ] Low priority: Abandoned-checkout status (initiated but didn't complete)
- [ ] Low priority: Checkout-initiated status (started checkout)
- [ ] Low priority: Registered status (has account)
- [ ] Lowest priority: New status (default state)

## Key Assumptions

- [ ] Redis connection is available and configured properly
- [ ] User object contains necessary identification to perform stage determination
- [ ] The funnel stage determination functions handle all edge cases properly
- [ ] External integrations (SendGrid) properly receive and process updates
- [ ] Errors in queue processing are properly logged but don't crash the application
- [ ] The priority ordering of funnel stages accurately reflects business requirements
- [ ] Stage changes need to be timestamped for accurate user journey analytics
- [ ] Queue system provides sufficient reliability for this marketing categorization

## Usage Notes

The queue is typically invoked by other parts of the system when user state changes occur, such as:

- User registration
- Subscription changes
- Trial starts/ends
- Session activity tracking
- Checkout events

## Error Handling

The queue process handler implements try/catch error handling to prevent queue failures from affecting the main application. All errors during processing are logged but allowed to resolve successfully to maintain queue stability.

## Related Components

- `utils/funnelstages/funnelStages.js`: Core stage determination logic
- `userHelperFunctions/useFunnelStageDeterminer.js`: Wrapper for consistent stage resolution
- `resolvers/resolveUserFunnelStage.js`: Applies stage priority business rules