# Stripe-MongoDB Subscription Synchronization Validator

This utility provides a comprehensive way to validate the consistency between Stripe subscriptions and MongoDB user records. It helps identify accounts that may be out of sync between the two systems.

## Purpose

The Stripe-MongoDB synchronization validator serves the following purposes:

1. **Identify Plan Inconsistencies**: Find users who have different plan levels in Stripe versus MongoDB
2. **Identify Status Inconsistencies**: Find users whose subscription status doesn't match between systems
3. **Detect Missing Records**: Find users who have paid plans in one system but not the other
4. **Validate Customer IDs**: Ensure that the Stripe customer IDs are consistent between systems
5. **Generate Metrics**: Provide statistics on the overall health of the subscription sync system

## Usage

Run the utility using npm:

```bash
npm run confirm-stripe-db-sync
```

## How It Works

The script performs the following operations:

1. Connects to MongoDB using the environment variables
2. Fetches **all** subscriptions from Stripe's API (not just active ones)
3. Fetches all users with paid plans from MongoDB
4. Processes multiple subscriptions per customer to determine the most relevant one
5. Compares subscription plan IDs and statuses between systems
6. Generates a detailed report of all discrepancies found
7. Provides metrics on the health of the synchronization

## Types of Inconsistencies Detected

The script identifies several types of inconsistencies:

1. **`plan_id_mismatch`**: User has a different plan level in MongoDB than in Stripe
2. **`status_mismatch`**: The subscription status in MongoDB doesn't match the status in Stripe
3. **`missing_in_stripe`**: User has a paid plan in MongoDB but no corresponding subscription in Stripe
4. **`missing_in_mongodb`**: User has an active subscription in Stripe but no paid plan in MongoDB
5. **`customer_id_mismatch`**: The Stripe customer ID in MongoDB doesn't match the one in Stripe

## Subscription Status Handling

The script intelligently handles subscription statuses:

- For Stripe, the following statuses are considered "active": `['active', 'trialing', 'past_due']`
- When a user has multiple subscriptions, the script:
  1. First prioritizes subscriptions with active statuses
  2. Then prioritizes non-canceled subscriptions
  3. Finally falls back to the most recently created subscription of any status

## Example Output

```
========== INCONSISTENCY REPORT ==========
Found 6 inconsistencies between Stripe and MongoDB.

----- Summary by Type -----
plan_id_mismatch: 2 issues
status_mismatch: 1 issues
missing_in_stripe: 2 issues
missing_in_mongodb: 1 issues

----- Detailed Report -----

Issue #1 (plan_id_mismatch):
Email: user1@example.com
MongoDB Plan: 3 (Premium Monthly)
Stripe Plan: 2 (Basic Monthly)
Stripe Status: active
Description: User with email user1@example.com has plan 3 (Premium Monthly) in MongoDB but plan 2 (Basic Monthly) in Stripe.

Issue #2 (status_mismatch):
Email: user2@example.com
MongoDB Plan: 2 (Basic Monthly)
Stripe Plan: 2 (Basic Monthly)
Stripe Status: past_due
MongoDB Status: canceled
Description: User with email user2@example.com has canceled status in MongoDB but past_due status in Stripe.

----- Metrics Summary -----
Stripe Accounts Reviewed: 150
MongoDB Users Reviewed: 125
MongoDB Subscriptions Reviewed: 140

Errors by Type:
  plan_id_mismatch: 2
  status_mismatch: 1
  missing_in_stripe: 2
  missing_in_mongodb: 1
  customer_id_mismatch: 0

Total Errors: 6
Error Rate: 4.80% of MongoDB users have inconsistencies

========== END OF REPORT ==========
```

## Important Notes

1. This script is **READ-ONLY** and does not modify any data in either system
2. It requires appropriate Stripe API credentials in your environment variables
3. For large numbers of users/subscriptions, the script may take several minutes to run
4. The script handles pagination for Stripe API results to ensure all subscriptions are checked

## Understanding Multiple Subscriptions

A customer may have multiple subscriptions in Stripe at the same time. When this happens, the script uses the following hierarchy to determine which subscription is most relevant:

1. Active status subscriptions (active, trialing, past_due)
2. Non-canceled subscriptions of any other status
3. Most recently created subscription of any status

This ensures that if a customer has both an active and canceled subscription, the active one will be used for comparison.

## Troubleshooting

If you encounter issues running the script:

1. Ensure your Stripe API key is correctly set in your environment variables
2. Check that your MongoDB connection string is valid
3. Verify you have the required permissions to access both Stripe and MongoDB data

## Fixing Inconsistencies

After identifying inconsistencies, you may need to:

1. For `plan_id_mismatch`: Update the user's currentPlan field in MongoDB to match Stripe
2. For `status_mismatch`: Update the subscription status in MongoDB to match Stripe
3. For `missing_in_stripe`: Investigate why MongoDB shows a paid plan with no Stripe subscription
4. For `missing_in_mongodb`: Create or update user records in MongoDB for customers with active Stripe subscriptions
5. For `customer_id_mismatch`: Update the stripeCustomerId in MongoDB to match Stripe

For specific resolution steps, consult with your development and billing teams.