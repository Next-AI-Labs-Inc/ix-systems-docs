# Slack Notification System

This document explains the proper usage of Slack notifications in the IX Coach API.

## Two Separate Systems

The IX Coach platform uses two separate systems for Slack notifications:

1. **Direct User Notifications (`SlackNotifier.js`)** - For direct user communications with @mentions
2. **Automated System Messages (`logger.js`)** - For automated system messages without @mentions

## When to Use Each System

### Use `SlackNotifier.js` ONLY for:

- **Human-requested direct notifications** to team members (e.g., "notify @gabriel")
- When Claude needs to **notify a specific person** (via `notifyGabriel`, `notifyJonathan`, etc.)
- Messages that require **immediate attention with @mentions**
- Messages sent to the **core-dev channel with @mentions**

```javascript
// CORRECT: When a human asks to notify someone
const { notifyGabriel } = require('./utils/SlackNotifier');
notifyGabriel('Please review this PR');
```

### Use `logger.js` for:

- **All automated system messages** (info, warnings, errors)
- **Logging events** without @mentions
- Messages that should go to the **dedicated info/warning/error channels**
- Any notification that **doesn't need an @mention**

```javascript
// CORRECT: For automated system messages
const logger = require('./utils/logger');
logger.info('New user registered');
logger.warn('API rate limit approaching');
logger.error('Database connection failed');
```

## IMPORTANT RULES

1. **NEVER use SlackNotifier.js for automated messages** - this spams the core-dev channel
2. **ALWAYS use logger.js for system info/warning/error messages**
3. **ONLY use core-dev with @mentions**, never for automated logging
4. **SlackNotifier.js is for direct team communications with @mentions ONLY**

## Environment Configuration

The two systems use different configuration:

- **SlackNotifier.js**: Uses `SLACK_BOT_TOKEN` and direct channel IDs
- **logger.js**: Uses webhook URLs via environment variables:
  - `SLACK_INFO_WEBHOOK_URL`
  - `SLACK_WARN_WEBHOOK_URL`
  - `SLACK_ERROR_WEBHOOK_URL`

## Examples

### Correct Usage

```javascript
// CORRECT: User notification with @mention (HUMAN-REQUESTED)
notifyGabriel('Need your review on PR #123');

// CORRECT: Automated system messages (NO @MENTIONS)
logger.info('User upgraded to premium plan');
logger.warn('User approaching API rate limit');
logger.error('Payment processing failed');
```

### Incorrect Usage

```javascript
// WRONG: Sending automated message to core-dev
sendChannelMessage('New user registered', CHANNEL_IDS['core-dev']);

// WRONG: Using SlackNotifier for automated messages
sendInfoMessage('Database backup completed'); // Should use logger.info()

// WRONG: Using logger for direct user communications
logger.info('@gabriel Please check this code'); // Should use notifyGabriel()
```

## Channel Usage

- **core-dev channel**: ONLY for team communications with @mentions
- **info channel**: Automated system events, user actions, operations
- **warning channel**: Automated potential issues, approaching limits
- **error channel**: Automated system failures, errors, issues

## Testing the Systems

To verify both systems are working:

```javascript
// Test direct notification (will go to core-dev with @mention)
const { notifyGabriel } = require('./utils/SlackNotifier');
notifyGabriel('Testing direct notification');

// Test automated system message (will go to info channel via webhook)
const logger = require('./utils/logger');
logger.info('Testing automated system message');
```