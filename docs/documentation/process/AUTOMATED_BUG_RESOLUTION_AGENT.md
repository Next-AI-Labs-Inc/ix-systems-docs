# Automated Bug Resolution Agent System

## Overview

This document outlines the implementation plan for automating the Bug Resolution Agent to run automatically when errors are detected without manual CLI invocation. The system will perform multi-step analysis (5-15 steps), implement high-confidence fixes, and notify team members following our established protocols.

## Background

Currently, the Bug Resolution Agent (#32) must be manually triggered via command line when errors are encountered. We need a system that automatically:

1. Detects errors through our logging system
2. Triggers the Bug Resolution Agent analysis
3. Performs a thorough multi-step investigation
4. Implements fixes for high-confidence cases
5. Creates GitHub issues with full context
6. Notifies appropriate team members via Slack

## Technical Requirements

### 1. Error Detection & Webhook Integration

- **Error Webhook Endpoint**
  - Create new endpoint at `/api/agent/bug-resolution`
  - Accept error payloads with message, stack, context
  - Implement proper authentication

- **Logger Enhancement**
  - Modify `/utils/logger.js` to forward critical errors
  - Add severity classification system
  - Integrate with existing Sentry reporting

```javascript
// Example logger enhancement
logger.error = function(message, errorObj = {}, options = {}) {
  // Execute normal error logging
  this.log({
    level: 'error',
    message: message
  });
  
  // Check if this error should trigger agent analysis
  if (options.triggerAnalysis || isCriticalError(message, errorObj)) {
    // Forward to agent webhook
    triggerBugResolutionAgent({
      message,
      stack: errorObj.stack,
      context: options.context || {},
      source: options.source || 'logger'
    });
  }
};
```

### 2. Agent Runner Service

- **Agent Invocation Module**
  - Create `utils/agentRunner.js`
  - Implement secure execution of CLI commands
  - Properly pass error context to agent

```javascript
// utils/agentRunner.js
const { execSync } = require('child_process');
const logger = require('./logger');

async function invokeBugResolutionAgent(errorData) {
  // Format input data
  const encodedData = Buffer.from(JSON.stringify(errorData)).toString('base64');
  
  // Execute agent with proper environment
  const command = `cd /Users/jedi/react_projects/ix && claudecode agent:bug fix --error-data=${encodedData}`;
  
  try {
    const result = execSync(command);
    return { success: true, output: result.toString() };
  } catch (error) {
    logger.error(`Error invoking Bug Resolution Agent: ${error.message}`);
    return { success: false, error: error.message };
  }
}

// Provide entry points for various error sources
module.exports = {
  invokeBugResolutionAgent,
  processLoggerError: (message, stack, context) => {
    return invokeBugResolutionAgent({ message, stack, context, source: 'logger' });
  },
  processBugReport: (bugReport) => {
    return invokeBugResolutionAgent({ 
      message: bugReport.description, 
      steps: bugReport.steps,
      actualBehavior: bugReport.actualBehavior,
      expectedBehavior: bugReport.expectedBehavior,
      source: 'bug_report'
    });
  },
  processWebhookError: (webhookData) => {
    return invokeBugResolutionAgent({
      message: webhookData.message,
      stack: webhookData.stack,
      context: webhookData.context,
      source: 'webhook'
    });
  }
};
```

### 3. Multi-step Analysis Queue

- **Analysis Queue System**
  - Implement Redis/Bull queue for analysis steps
  - Create progression tracking
  - Handle retries and failures

```javascript
// Example queue setup in /queues/analysisQueue.js
const Queue = require('bull');
const { REDIS_URL } = require('../config');

const analysisQueue = new Queue('bugResolutionAnalysis', REDIS_URL, {
  defaultJobOptions: {
    attempts: 3,
    backoff: {
      type: 'exponential',
      delay: 5000
    }
  }
});

// Process jobs
analysisQueue.process(async (job) => {
  const { errorData, currentStep, results } = job.data;
  
  try {
    // Execute the current analysis step
    const stepResult = await executeAnalysisStep(currentStep, errorData, results);
    
    // Store results
    const updatedResults = {
      ...results,
      [currentStep]: stepResult
    };
    
    // Determine next step
    const nextStep = determineNextStep(currentStep, stepResult);
    
    if (nextStep) {
      // Queue next step
      await analysisQueue.add({
        errorData,
        currentStep: nextStep,
        results: updatedResults
      });
      
      return { success: true, nextStep, results: updatedResults };
    } else {
      // Analysis complete - process final results
      return processFinalResults(errorData, updatedResults);
    }
  } catch (error) {
    logger.error(`Error in analysis step ${currentStep}: ${error.message}`);
    throw error;
  }
});

module.exports = {
  startAnalysis: (errorData) => {
    return analysisQueue.add({
      errorData,
      currentStep: 'parse_error',
      results: {}
    });
  }
};
```

### 4. Error Context Collection

- **Express Middleware**
  - Add middleware to capture request context
  - Include in error reports
  - Save system state snapshot

```javascript
// errorContextMiddleware.js
function errorContextMiddleware(req, res, next) {
  // Create request context container
  req.errorContext = {
    url: req.originalUrl,
    method: req.method,
    timestamp: new Date().toISOString(),
    headers: req.headers,
    query: req.query,
    userAgent: req.get('User-Agent'),
    userId: req.user?._id || null,
    sessionId: req.sessionID || null
  };
  
  // Override error logging to include context
  const originalError = req.logger?.error || logger.error;
  req.logger = {
    ...req.logger,
    error: (message, errorObj = {}, options = {}) => {
      return originalError(message, errorObj, { 
        ...options, 
        context: req.errorContext 
      });
    }
  };
  
  next();
}

module.exports = errorContextMiddleware;
```

### 5. GitHub Integration

- **GitHub API Integration**
  - Enhance existing GitHub webhook integration
  - Add proper authentication for agent
  - Create consistent issue and PR formats

```javascript
// example GitHub API integration
const { Octokit } = require('@octokit/rest');
const octokit = new Octokit({ auth: process.env.GITHUB_TOKEN });

async function createGitHubIssue(analysisResults) {
  const { errorMessage, rootCause, fixApproach, confidence } = analysisResults;
  
  const issueBody = `
## Bug Analysis

**Error**: ${errorMessage}

**Root Cause**: ${rootCause}

**Fix Approach**: ${fixApproach}

**Confidence Level**: ${confidence}%

## Analysis Steps

${formatAnalysisSteps(analysisResults.steps)}

## System Context

${formatSystemContext(analysisResults.context)}
  `;
  
  const response = await octokit.issues.create({
    owner: 'Next-AI-Labs-Inc',
    repo: 'ixcoach-api',
    title: `Bug: ${errorMessage.slice(0, 100)}`,
    body: issueBody,
    labels: ['bug', 'automated-analysis', `confidence-${getConfidenceLabel(confidence)}`]
  });
  
  return response.data.html_url;
}
```

### 6. Slack Notification Integration

- **Extend Slack Notifier**
  - Use existing `/utils/slackNotifier.js`
  - Add agent-specific notification templates
  - Support interactive approval buttons

```javascript
// Add to slackNotifier.js
async function notifyAboutBugAnalysis(username, analysisResults, githubIssueUrl) {
  const { errorMessage, rootCause, fixApproach, confidence } = analysisResults;
  
  const message = `
I've analyzed a bug and determined the root cause:

*Error*: ${errorMessage}

*Root Cause*: ${rootCause}

*Fix Approach*: ${fixApproach}

*Confidence*: ${confidence}%

*GitHub Issue*: ${githubIssueUrl}

${confidence > 90 ? 'I\'ve created a PR with the fix. Please review.' : 'Please review the analysis and let me know if I should implement the fix.'}
  `;
  
  return await notifyUser(username, message, 'Bug Resolution Agent');
}
```

### 7. Analysis Status Dashboard

- **Admin UI Component**
  - Create React component for admin dashboard
  - Show active/past analyses
  - Allow manual intervention

### 8. Configuration System

- **Agent Configuration Module**
  - Create config for automated triggering
  - Set confidence thresholds
  - Configure recipient mapping

```javascript
// config/bugResolutionAgent.js
module.exports = {
  // Global enable/disable toggle
  enabled: process.env.ENABLE_BUG_RESOLUTION_AGENT === 'true',
  
  // Confidence thresholds
  confidenceThresholds: {
    autoFix: 95, // Only auto-fix above this confidence
    notification: 70, // Only notify above this confidence
    githubIssue: 50 // Only create GitHub issues above this confidence
  },
  
  // Error patterns that trigger analysis
  triggerPatterns: [
    'Schema hasn\'t been registered for model',
    'cannot read property .* of undefined',
    'is not a function',
    'Cannot find module',
    'ECONNREFUSED'
  ],
  
  // Team member mapping by error type
  teamMapping: {
    'schema': 'rasel',
    'database': 'rasel',
    'api': 'gabriel',
    'frontend': 'jonathan',
    'default': 'jonathan'
  },
  
  // Analysis steps configuration
  analysisSteps: {
    maxSteps: 15,
    minSteps: 5,
    timeout: 30000, // ms per step
  }
};
```

### 9. Error Pattern Learning

- **Knowledge Base System**
  - Create MongoDB model for storing patterns
  - Implement pattern matching
  - Learn from successful fixes

```javascript
// models/ErrorPattern.js
const mongoose = require('mongoose');
const { Schema } = mongoose;

const ErrorPatternSchema = new Schema({
  pattern: {
    type: String,
    required: true,
    index: true
  },
  exampleError: {
    type: String,
    required: true
  },
  rootCause: {
    type: String,
    required: true
  },
  fixApproach: {
    type: String,
    required: true
  },
  confidence: {
    type: Number,
    default: 50
  },
  successCount: {
    type: Number,
    default: 0
  },
  failureCount: {
    type: Number,
    default: 0
  },
  lastUsed: {
    type: Date,
    default: Date.now
  },
  fixCode: String,
  reportedBy: String,
  createdAt: {
    type: Date,
    default: Date.now
  }
});

module.exports = mongoose.model('ErrorPattern', ErrorPatternSchema);
```

## Implementation Plan

### Phase 1: Foundation (2 weeks)

1. Create error webhook endpoint
2. Implement agent runner service
3. Enhance logger integration
4. Basic configuration system

### Phase 2: Analysis System (2 weeks)

1. Multi-step analysis queue
2. Error context collection
3. GitHub issue integration
4. Slack notification integration

### Phase 3: Smart Features (3 weeks)

1. Error pattern learning
2. Knowledge base system
3. Confidence scoring
4. Analysis dashboard

### Phase 4: Automations (1 week)

1. Automatic fix implementation for high-confidence cases
2. Approval workflow for medium-confidence cases
3. A/B testing framework for multiple fix approaches

## Acceptance Criteria

1. System automatically detects errors and triggers analysis
2. Multi-step analysis process thoroughly investigates errors
3. GitHub issues are created with detailed analysis
4. Team members are notified via Slack following protocol
5. High-confidence fixes are implemented automatically
6. System learns from past fixes to improve
7. Admin dashboard shows analysis status and history

## Additional Notes

- This system should operate within our existing error handling and logging infrastructure
- All automated actions should be fully traceable
- The system should require minimal configuration
- Focus initially on server-side errors in the API and expand to include frontend errors later