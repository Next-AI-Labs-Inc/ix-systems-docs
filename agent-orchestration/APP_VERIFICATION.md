# IXCoach API Verification Scripts

This documentation outlines the verification scripts created for the IXCoach API. These scripts help ensure that the application is running correctly and maintains good performance.

## Table of Contents

- [For AI Agents](#for-ai-agents)
- [Verification Scripts](#verification-scripts)
- [Usage](#usage)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Integrating into Your Workflow](#integrating-into-your-workflow)

## For AI Agents

> **IMPORTANT**: AI Agents should run the verification scripts after making significant changes to the codebase to ensure they haven't introduced issues that could crash the application.

The `agent-app-health-check.js` script is specifically designed for AI agents to quickly determine if the API is functioning correctly.

```bash
node scripts/agent-app-health-check.js
```

This script will:
1. Check if the API is running
2. Perform basic health checks on key endpoints
3. Detect common errors and issues
4. Provide specific recommendations for fixes if problems are found

### Recommended Agent Workflow

1. Make your code changes
2. Run the agent health check to verify the API still works
3. If issues are detected, review the recommendations and fix the problems
4. Verify again after fixing issues

### Example Response Interpretation

```
===== IX COACH API HEALTH CHECK RESULTS =====
App Status: ERROR
URL: http://localhost:4000

ERRORS:
• Connection error: ECONNREFUSED
• The API is not running or responding correctly

RECOMMENDED ACTIONS:
• Start the API with "npm start" or "npm run dev"
• Check terminal output for startup errors
• Check server logs for details
```

In this example, the agent should check why the API isn't running, possibly due to syntax errors they introduced.

## Verification Scripts

The system includes two verification scripts:

1. **verify-app-health.js** - Comprehensive verification of API health
2. **agent-app-health-check.js** - Quick health check designed for AI agents

### Script Details

#### verify-app-health.js

Performs a comprehensive health check of the IXCoach API, verifying endpoints, database connection, and environment variables.

```bash
node scripts/verify-app-health.js
```

#### agent-app-health-check.js

Quick health check designed specifically for AI agents with focused feedback.

```bash
node scripts/agent-app-health-check.js
node scripts/agent-app-health-check.js --port=3001  # Custom port
node scripts/agent-app-health-check.js --json       # JSON output
node scripts/agent-app-health-check.js --verbose    # Verbose output
```

## Usage

### Basic Usage

To run the full health check:

```bash
node scripts/verify-app-health.js
```

For a quick check of API health:

```bash
node scripts/agent-app-health-check.js
```

### Interpreting Results

All verification scripts provide detailed output with:

- Overall pass/fail status
- Specific issues detected (if any)
- Recommendations for fixing problems
- Detailed logs for troubleshooting

### Log Files

Each verification script generates a log file in the project root:

- `.app-health-check.log` - Main verification log
- `.agent-health-check.log` - Agent health check log

## Troubleshooting Common Issues

### API Won't Start

If the API won't start, common issues include:

1. **Syntax errors**: Check recent code changes for syntax issues
2. **Import errors**: Ensure all imported modules exist
3. **Port conflicts**: Another process might be using port 4000
4. **Missing dependencies**: Run `npm install` to ensure all dependencies are installed

### API Responds with Errors

Common API issues include:

1. **Database connection errors**: Check database credentials and connection
2. **Environment variable issues**: Missing required environment variables
3. **Auth errors**: JWT secret or auth configuration issues
4. **Memory issues**: The application may be running out of memory

## Integrating into Your Workflow

### For AI Agents

AI agents should:

1. Run health checks after making significant changes
2. Include verification results in their work summaries
3. Document any fixes needed due to verification failures

### Example Workflow Checklist

- [ ] Make code changes
- [ ] Run `node scripts/agent-app-health-check.js`
- [ ] Fix any issues detected
- [ ] Verify fixes with another health check
- [ ] Document verification results
- [ ] Commit changes only when verification passes

## Conclusion

The verification scripts provide a robust system for ensuring the IXCoach API remains stable and functional. By integrating these checks into your workflow, you can catch issues early and maintain high code quality.