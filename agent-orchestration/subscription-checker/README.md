# Subscription Status Checker

A tool to identify and debug subscription synchronization issues between MongoDB and Stripe.

## Purpose

This tool helps support staff diagnose when users report payment or access problems by:

1. Revealing discrepancies between MongoDB and Stripe subscription data
2. Detecting specific issues like status mismatches and missing subscriptions
3. Generating detailed reports for troubleshooting

## Usage

There are two ways to use this tool:

1. Using npm script:
```bash
npm run confirm-stripe-db-sync-2-0 -- <email>
```
(Note the `--` which is required to pass arguments to npm scripts)

2. Using the CLI script directly:
```bash
./scripts/confirm-stripe-db-sync-2-0 <email>
```

If you run the command without an email, it will prompt you to enter one:
```bash
npm run confirm-stripe-db-sync-2-0
```

## Output Format

The tool provides both human-readable output and machine-readable JSON with the following information:

1. User Information (email, MongoDB plan, Stripe ID)
2. MongoDB Subscription Data
3. Stripe Subscription Data
4. Subscription Matching Analysis
5. Issue Summary
6. Machine Readable Output with issue tags

## Issue Tags

The tool can detect and tag the following issues:

- `missing_in_stripe`: User has a paid plan in MongoDB but no subscription in Stripe
- `trial_status_mismatch`: MongoDB shows 'active' but Stripe shows 'trialing'
- `status_mismatch`: Other status mismatches between MongoDB and Stripe
- `multiple_mongo_subscriptions`: User has multiple subscriptions in MongoDB
- `multiple_stripe_subscriptions`: User has multiple subscriptions in Stripe
- `id_mismatch`: Subscription IDs don't match between systems
- `active_during_trial`: User has an active MongoDB subscription during Stripe trial period

## Modular Design

The tool is split into several modules:

- `config.js`: Configuration settings
- `db.js`: MongoDB connection and queries
- `stripe.js`: Stripe API operations
- `formatter.js`: Output formatting for readability
- `index.js`: Main subscription checking logic

## Example Issues

Common issues detected:

1. User with active status in MongoDB but trialing in Stripe
2. User with a paid plan in MongoDB but no subscription in Stripe
3. Multiple subscriptions in either system
4. Subscription ID mismatch between systems