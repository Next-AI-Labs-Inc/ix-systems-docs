# IX Coach API Directory Structure Map

This map shows the directory structure of the IX Coach API with intention statements for each directory. Items marked with [2.0] represent the new separation of concerns architecture.

```
/Users/jedi/react_projects/ix/ixcoach-api/ (Root of API for core server logic)
├── areaCodes/ (Localization code tables for country/state lookup)
├── assets/ (Static resources for email templates and logos)
├── backups/ (Database restoration points for critical fixes)
├── components/Auth/ (Authentication UI components)
├── config/ (System configuration for CORS, rate limits, integrations)
├── controllers/ (Request handlers that process API business logic)
│   ├── admin/ (Administration panel backend logic)
│   ├── externalApi/ (GitHub and third-party integration endpoints)
│   ├── journal-app/ (Note-taking feature backend handlers)
│   ├── project/ (Project management functionality)
│   ├── referrals/ (User referral program handlers)
│   └── subscription/ (Legacy payment flow controllers)
├── database-operations/ [2.0] (Place new DB operations for atomicity)
│   ├── __tests__/ [2.0] (Test suite for database operations)
│   ├── docs/ [2.0] (Documentation for data schema and operations)
│   ├── entity-resolvers/ [2.0] (Business entity extraction from DB)
│   ├── migrations/ [2.0] (Schema and data transformation scripts)
│   ├── operations/ [2.0] (Atomic database transaction operations)
│   ├── persistence/ [2.0] (Data access layer for consistent storage)
│   ├── queries/ [2.0] (Optimized MongoDB query operations)
│   ├── tests/ [2.0] (Integration and unit tests for DB ops)
│   ├── transactions/ [2.0] (Multi-operation atomic transactions)
│   └── utils/ [2.0] (DB operation helper functions)
├── docs/ (API documentation and change records)
│   ├── change-review/ (PR review standards and guidelines)
│   ├── changelog/ (Release notes and version history)
│   ├── initiatives/ (Strategic development roadmap documents)
│   ├── issues/ (Problem documentation and root cause analysis)
│   ├── proposed/ (Potential features under consideration)
│   ├── stripe/ (Payment system integration guidance)
│   └── subscription/ (Billing system documentation)
├── emailTemplates/ (HTML/JS email templates for notifications)
├── examples/ (Code samples for documentation)
├── knowledge-files/ (User knowledge base for Claude/AI training)
├── middlewares/ (Express middleware for auth, validation, error handling)
├── mocks/ (Test fixtures for simulating external services)
├── models/ (MongoDB schemas defining database structure)
│   ├── admin/ (Admin panel data models)
│   ├── ai_training/ (AI training data storage schemas)
│   ├── journal-app/ (Note-taking feature data models)
│   ├── project/ (Project management data structures)
│   └── subscription/ (Payment plan and feature access models)
├── plugins/ (Extensibility system for custom features)
├── public/ (Publicly accessible static resources)
├── queues/ (Background job processing for emails, analytics)
├── routes/ (API endpoint definitions and routing)
│   ├── admin/ (Admin dashboard API routes)
│   ├── auth/ (Authentication endpoint definitions)
│   ├── externalApi/ (External integrations API routes)
│   ├── journal-app/ (Note-taking feature endpoints)
│   ├── project/ (Project management API routes)
│   ├── referrals/ (User referral program endpoints)
│   └── slack/ (Slack integration API routes)
├── scheduler/ (Scheduled tasks for maintenance, reminders)
├── scripts/ (Maintenance, deployment, and utility scripts)
├── stripe-api-client/ [2.0] (Place new Stripe API interactions)
│   ├── __tests__/ [2.0] (Test suite for Stripe client)
│   ├── customer/ [2.0] (Customer management operations)
│   ├── docs/ [2.0] (Stripe integration documentation)
│   ├── invoice/ [2.0] (Invoice creation and management)
│   ├── payment/ [2.0] (Payment processing operations)
│   ├── plan/ [2.0] (Subscription plan definitions)
│   ├── product/ [2.0] (Product catalog management)
│   ├── subscription/ [2.0] (Subscription lifecycle management)
│   ├── tests/ [2.0] (Integration tests with Stripe API)
│   └── utils/ [2.0] (Helper functions for Stripe operations)
├── subscriptions/ (Legacy subscription management)
├── subscriptions-and-stripe-webhooks/ [2.0] (Place new webhook handlers)
│   ├── __tests__/ [2.0] (Test suite for webhook processing)
│   ├── docs/ [2.0] (Webhook documentation and flow diagrams)
│   ├── entity-resolvers/ [2.0] (Entity extraction from webhook data)
│   ├── event-handlers/ [2.0] (Event-specific webhook processors)
│   ├── handlers/ [2.0] (Core webhook processing logic)
│   ├── migrations/ [2.0] (Subscription data migration tools)
│   ├── mocks/ [2.0] (Test fixtures for webhook simulation)
│   ├── persistence/ [2.0] (Webhook data storage operations)
│   ├── resolvers/ [2.0] (User/subscription state resolvers)
│   ├── services/ [2.0] (Business logic services for webhooks)
│   ├── side-effects/ [2.0] (Secondary operations after processing)
│   ├── state-determiners/ [2.0] (State calculation from webhook data)
│   ├── tests/ [2.0] (Integration and unit tests)
│   ├── utils/ [2.0] (Helper functions for webhook processing)
│   └── validators/ [2.0] (Webhook data validation rules)
├── swagger-components/ (OpenAPI/Swagger documentation schemas)
├── templates/ (Server-side templates for reporting)
├── test-utils/ (Testing utility functions and reporters)
├── tests/ (Test suites for API functionality)
├── utils/ (Shared utility functions)
│   ├── agent-task-functions/ (Task tracking system for AI agents)
│   ├── ai/ (AI integration for chat and analysis)
│   ├── aiCoachBuilder/ (AI coaching prompt construction)
│   ├── auth/ (Authentication utility functions)
│   ├── email/ (Email sending and templating utilities)
│   ├── firebase/ (Firebase integration utilities)
│   ├── funnelstages/ (User journey tracking utilities)
│   ├── loggers/ (Logging infrastructure for system events)
│   └── meta/ (Facebook Meta tracking integration)
└── vectorStoreDirectory/ (Vector embeddings for AI knowledge)
    ├── 670de5a5c0bd6d6bbee1c429/ (User-specific knowledge embeddings)
    └── coaching/ (AI coaching knowledge embeddings)
        ├── global/ (Global knowledge for all users)
        └── project/ (Project-specific knowledge vectors)
```

## Slack Notification System Files

These files handle team notifications and follow a separation of concerns pattern:

- `/utils/SlackNotifier.js` - Team notification system for user requests
- `/utils/slackNotifier.README.md` - Usage guide for notification system

## 2.0 Architecture Overview

The 2.0 labeled directories represent the new separation of concerns architecture with these design principles:

1. **Clean separation** - Each module has a single responsibility
2. **Atomic operations** - Database operations are transactional
3. **Testable interfaces** - All components have dedicated test suites
4. **Documentation-first** - Each module includes detailed docs
5. **Domain-driven design** - Organization follows business domains

When implementing new features:

- For database operations, use `/database-operations/`
- For Stripe API interactions, use `/stripe-api-client/`
- For webhook handling, use `/subscriptions-and-stripe-webhooks/`
