# Agent Code Placement Guide

## Core Domains

### stripe-api-client/
Place all direct interactions with the Stripe API in this directory:

- **stripe-api-client/customer/** - Functions that create, update, or retrieve Stripe customers
- **stripe-api-client/subscription/** - Functions that manage Stripe subscriptions (create, update, cancel)
- **stripe-api-client/payment/** - Functions that process payments or issue refunds
- **stripe-api-client/invoice/** - Functions that create or pay invoices
- **stripe-api-client/product/** - Functions that manage Stripe products and prices
- **stripe-api-client/plan/** - Functions that interact with pricing plans

### database-operations/
Place all database access functions in this directory:

- **database-operations/entity-resolvers/** - Functions that retrieve entities by ID, email, or other identifiers
- **database-operations/operations/** - Basic CRUD operations for each entity type
- **database-operations/queries/** - Complex queries involving multiple entities
- **database-operations/transactions/** - Functions that perform atomic operations across multiple entities
- **database-operations/persistence/** - Functions that save entity state changes to the database

### subscriptions-and-stripe-webhooks/
Place all webhook handling code in this directory (existing structure):

- **subscriptions-and-stripe-webhooks/event-handlers/** - Entry points for specific webhook event types
- **subscriptions-and-stripe-webhooks/validators/** - Functions that validate webhook payload structure 
- **subscriptions-and-stripe-webhooks/entity-resolvers/** - Functions that look up entities referenced in webhooks
- **subscriptions-and-stripe-webhooks/state-determiners/** - Pure functions that compute state changes
- **subscriptions-and-stripe-webhooks/persistence/** - Functions that apply computed changes to the database
- **subscriptions-and-stripe-webhooks/side-effects/** - Functions that trigger emails, notifications, etc.

## Testing Structure

Each domain should maintain consistent test structure:

- **[domain]/tests/unit/** - Unit tests for individual functions
- **[domain]/tests/integration/** - Tests that verify interactions between components
- **[domain]/tests/mocks/** - Mock data used in tests

## Documentation Structure

Maintain documentation for each domain:

- **[domain]/docs/changelog/** - Records of changes made to this domain
- **[domain]/docs/issues/** - Documentation of issues and their resolutions

## Guidelines for Agents

1. **NEVER** add new code to existing large files
2. **ALWAYS** place new functions in the appropriate domain directory
3. **NEVER** create functions that cross domain boundaries - instead, compose functions from different domains
4. **ALWAYS** create unit tests alongside new functions
5. **NEVER** modify existing code without first creating a replacement that passes tests