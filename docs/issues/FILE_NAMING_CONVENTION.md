# File Naming Convention for Issue Documentation

## Standard Format

All issue documentation files should follow this naming pattern:

```
YYYY-MM-DD-descriptive-name-in-lowercase.md
```

For example:
- `2025-04-16-webhook-data-extraction-issues.md`
- `2025-04-07-apple-email-blocking-issue.md`

## Rules

1. **Date First** - Start with the date in YYYY-MM-DD format
2. **All Lowercase** - Use lowercase for all words in the filename
3. **Hyphens as Separators** - Use hyphens to separate words
4. **Descriptive Names** - Choose clear, descriptive names that indicate the issue
5. **Markdown Extension** - Always use .md extension

## Directory Structure

Issues are organized in the following directories:

- `/issues/` - General issues
- `/issues/webhook-issues/` - Webhook-specific issues
- `/issues/api_test_issues/` - API test-related issues
- `/issues/login/` - Authentication-related issues
- `/issues/test_failures/` - Test failure reports

When adding new subdirectories, use lowercase words with hyphens (e.g., `webhook-issues`).

## Index Files

Two main index files track all issues:

- `index.md` - Simple categorical listing of issues
- `index-map.md` - Comprehensive tracking with status and intent

Both should be maintained in lowercase.