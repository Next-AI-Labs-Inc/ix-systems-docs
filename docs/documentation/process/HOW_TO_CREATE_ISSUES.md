# How to Create Effective User-Centric Issue Documentation

This guide outlines the standardized approach for documenting issues in a way that connects technical problems to their user experience impact. Following these guidelines ensures all team members understand not just what's broken, but why it matters to our users.

## Core Principles

1. **User-first perspective**: Focus on how the issue affects the user experience, not just the technical details
2. **Conversational clarity**: Use direct, conversational language without unnecessary jargon
3. **Comprehensive context**: Include both user impact and technical details needed for resolution
4. **Actionable information**: Provide specific code paths and functions affected

## Required Format

Each issue document should follow this structure:

```markdown
# [User Experience Area]: [Specific User Flow Affected]

[1-2 paragraphs explaining the issue from a user perspective and technical cause]

[1 paragraph explaining why this specific test or feature is important in user terms]

[1 paragraph with additional context or edge cases that clarify the importance]

**Affected code**: `path/to/file.js` (function: `specificFunction`), `path/to/model.js`, `path/to/test.js`

**Detailed documentation**: `/docs/path/to/technical/details.md` (if applicable)
```

## Examples

### Example 1: Technical Bug with User Impact

#### ❌ Poor Documentation (Too Technical)

```markdown
# Webhook Route Configuration Issue

The webhook route in `routes/webhook.js` is registered at `/api/webhook` but tests are calling `/subscription/webhook`, causing 404 errors in test suite.

Need to update the paths to match or modify route registration.
```

#### ✅ Good Documentation (User-Centric)

```markdown
# Payment Error Recovery: Guiding Users Through Payment Issues

The webhook error handling tests are hitting 404 errors because they're using the wrong endpoint path. Tests are calling `/subscription/webhook` while the actual route is registered at `/api/webhook`.

These tests verify that users receive helpful guidance when payments fail. Instead of a generic "An error occurred" message, users should see specific reasons like "Your card was declined due to insufficient funds" with clear instructions on what to do next. The invalid signature test confirms we properly handle potential security issues, while still providing useful error information.

The error handling test ensures our webhook system is resilient - even if processing a specific payment event fails, the API should still acknowledge receipt to Stripe (preventing endless retries) while logging the error for our team to investigate.

**Affected code**: `routes/webhook.js`, `app.js` (middleware registration), `controllers/subscription/stripeController.js` (error handling in `stripeWebhook` function)

**Detailed documentation**: `/docs/issues/test_failures/WEBHOOK_ROUTE_ISSUE.md`
```

### Example 2: Database Issue with User Experience Focus

#### ❌ Poor Documentation (Too Vague)

```markdown
# MongoDB Timeouts in Tests

Database operations are timing out after 30 seconds in tests. Need to increase timeout or fix connection.
```

#### ✅ Good Documentation (User-Centric)

```markdown
# Email Notification Reliability: Contact System Performance

Email-related database operations are timing out after 30 seconds. The `personalizedpitchemailtemplates` collection and contact system queries are both failing to complete in time.

This prevents us from verifying that time-sensitive communications like password reset emails and subscription confirmations are delivered promptly. For password resets especially, delays can leave users locked out of their accounts. The template system is trying to auto-create default templates during tests, causing performance issues.

The contact system timeouts indicate potential performance problems when scaling up to many users. In production, these slowdowns would affect all email-based features, including onboarding flows, security notifications, and marketing communications.

**Affected code**: `models/PersonalizedPitchEmailTemplate.js` (function: `createDefaultIfNotExists`), `controllers/emailController.js` (function: `populateEmailv1foronce`), `models/Contact.js`

**Detailed documentation**: `/docs/issues/test_failures/DATABASE_CONNECTION_ISSUE.md` and `/docs/issues/test_failures/MONGOOSE_TIMEOUT_ISSUE.md`
```

### Example 3: Cross-Platform Experience Issue

#### ❌ Poor Documentation (Missing User Context)

```markdown
# Firebase User Sync Test Failures

MongoDB connection issues in Firebase user sync tests. The global `__MONGO_URI__` variable is undefined.
```

#### ✅ Good Documentation (User-Centric)

```markdown
# Cross-Platform Authentication Persistence: Account Synchronization

The Firebase user synchronization tests are failing with database connection errors. The MongoDB URI is undefined because the memory server isn't configured correctly.

This breaks our ability to verify that users can seamlessly move between platforms after changing their password. For example, when a user updates their password on the web app, our system should automatically sync this to Firebase, ensuring they can still log in on mobile without issues. This is particularly important after account migrations, where Firebase accounts might be missing entirely.

The test cleanup is also timing out after 5000ms, indicating connection management issues that could lead to resource leaks in the actual application.

**Affected code**: `__tests__/firebase-user-sync.test.js`, `controllers/authController.js` (function: `updatePassword`), `utils/firebaseAdmin.js`

**Detailed documentation**: `/docs/issues/test_failures/FIREBASE_SYNC_DATABASE_ISSUE.md`
```

## Key Improvements Checklist

When documenting issues, ensure you've made these key improvements:

✅ Replaced technical title with user experience focus  
✅ Explained why this issue matters to actual users  
✅ Included specific user workflows or journeys affected  
✅ Used conversational language throughout  
✅ Provided specific code paths and function names  
✅ Avoided vague statements like "if this broke..." in favor of direct impact descriptions  
✅ Connected technical failures to user-visible outcomes  
✅ Included both the symptom and underlying cause  

## Implementation Guidelines

1. Create issues in the appropriate repository's `docs/issues/` directory
2. Use descriptive filenames in UPPERCASE_WITH_UNDERSCORES.md format
3. Group related issues by area (api_issues, frontend_issues, etc.)
4. Include an INDEX.md file with a summary table of all issues
5. Link to more detailed technical documentation when available

By following these guidelines, we ensure that all team members—technical and non-technical alike—understand the real-world impact of issues and their priority for resolution.

---

*All done shutting down*