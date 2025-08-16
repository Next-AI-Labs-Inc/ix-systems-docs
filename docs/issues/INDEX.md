# IXCoach API Issues Index

This is NOT the definitive file for index of issue, this is /Users/jedi/react_projects/ix/ixcoach-api/docs/issues/INDEX-MAP.md

## Related Resources

- [Project Ecosystem Documentation](../../../docs/index.md)
- [IXCoach API Documentation](../index.md)
- [IXCoach API Changelog](../changelog/index.md)
- [GPTCoach2 Documentation](../../../gptcoach2/docs/README.md)
- [IX Tasks Documentation](../../../tasks/docs/index.md)

This directory contains documentation for key issues and their solutions in the IXCoach API.

## Current Issues

### Webhook Processing

- [Webhook Missing User Refactor Plan](./2025-04-09-webhook-missing-user-refactor-plan.md) - Technical implementation plan for fixing webhook handler crashes when user records are missing
- [Webhook Missing User Integration Plan](./2025-04-09-webhook-missing-user-integration-plan.md) - Deployment strategy for implementing the webhook handler missing user fix

### Subscription Management

- [Paid Users Treated Like Free Users](./2025-04-07-paid-users-treated-like-free-users.md) - Issue where paid users occasionally lose access to premium features

### Authentication

- [Firebase Provider Issue](./Sign_in_bugs_Issue_Apr_2_2025/resolved/EMAIL_AUTH_MISMATCH_ISSUE.md) - Issue with email authentication not matching Firebase auth records
- [Authentication Issues Summary](./Sign_in_bugs_Issue_Apr_2_2025/agent_review/1_issue_analysis/AUTHENTICATION_ISSUES_SUMMARY.md) - Overview of authentication-related issues

### Data Synchronization

- [MongoDB Firebase Sync Issue](./2025-04-07-mongodb-firebase-sync-issue.md) - Issues with data synchronization between MongoDB and Firebase
- [Subscription Access Logging Mismatch](./2025-04-07-subscription-access-logging-mismatch.md) - Discrepancies in subscription access logging

## Resolved Issues

- [Apple Email Blocking Issue](./2025-04-07-apple-email-blocking-issue.md) - Fixed issue with Apple email domains being incorrectly blocked
- [Stripe Webhook Race Condition Fix](./2025-04-16-stripe-webhook-race-condition-fix.md) - Solution for duplicate webhook processing

## Current Work

- [User Engagement Menu Helper Text](./2025-04-16-user-engagement-menu-helper-text.md) - Improving CLI tool guidance for admin console usability
- [Violations of Code Principles Fix Plan](./2025-04-16-violations-of-code-principles-fix-plan.md) - Plan for addressing code quality issues

## Issue Categories

### API Tests

- [Cross Platform Authentication](./api_test_issues/CROSS_PLATFORM_AUTHENTICATION.md)
- [Email Notification Reliability](./api_test_issues/EMAIL_NOTIFICATION_RELIABILITY.md)
- [Payment Error Recovery](./api_test_issues/PAYMENT_ERROR_RECOVERY.md)
- [Payment Workflow Reliability](./api_test_issues/PAYMENT_WORKFLOW_RELIABILITY.md)

### Database Issues

- [Database Connection Issue](./test_failures/DATABASE_CONNECTION_ISSUE.md)
- [Mongoose Timeout Issue](./test_failures/MONGOOSE_TIMEOUT_ISSUE.md)
- [Redis Connection Issues](./test_failures/REDIS_CONNECTION_ISSUES.md)

### Webhook Issues

- [Webhook Route Issue](./test_failures/WEBHOOK_ROUTE_ISSUE.md)
- [Webhook Validation Issues](./test_failures/WEBHOOK_VALIDATION_ISSUES.md)
- [Webhook Missing User Issue](./2025-04-09-webhook-missing-user-refactor-plan.md) - (_New_)

## Contributing

When adding new issue documentation, please:

1. Create a clear, descriptive filename with the format: `YYYY-MM-DD-descriptive-name-in-lowercase.md`
2. Follow the established format for issue documentation
3. Update this index with a link to your documentation
4. Categorize the issue appropriately
5. Cross-reference related issues

For complete file naming guidelines, see [File Naming Convention](./FILE_NAMING_CONVENTION.md).

A plan for renaming remaining files is available in [Renaming Plan](./RENAMING_PLAN.md).
