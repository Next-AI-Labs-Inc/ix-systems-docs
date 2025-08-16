# Slack Logger Utilities

This directory contains utilities for logging and channel management in Slack.

## Directory Structure

- `slack/`: Slack-specific logger implementations
  - `channels.js`: Channel management utilities
  - `messages.js`: Message handling utilities
  - `search.js`: Search functionality for Slack logs and channels
- `utils/`: Shared utilities used by loggers
  - `constants.js`: Shared constants like tokens and IDs
  - `formatters.js`: Message formatting utilities
- `examples/`: Example usage of the logger utilities
- `tests/`: Tests for logger functionality

## Usage

```javascript
const { searchChannels } = require('./loggers/slack/search');
const { listChannels } = require('./loggers/slack/channels');
```

## Security Note

All tokens should be loaded from environment variables, not hardcoded.
