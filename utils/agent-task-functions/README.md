# Agent Task Management Functions 🤖

> **[⬅️ BACK TO MAIN DOCUMENTATION](../../../CLAUDE.md)** | **[⬅️ QUICK REFERENCE](../../../claude_docs/REMINDER.md)**

KEYWORDS: agent functions, agent tools, agent API, task functions, task management, task tracking, agent task client, MongoDB tasks, agent utilities, agent task api, createTask, updateTask, getTask, getTasks, agent task management

This directory contains documentation and helper functions for managing tasks in the MongoDB-based task tracking system. These functions are essential for AI agents working within the IX ecosystem.

**IMPORTANT**: This documentation is the **CENTRAL AUTHORITY** for task management within the IX ecosystem. The MongoDB task tracking system is the central authority for managing all agent tasks in the IX ecosystem.

## Core Functions Overview

| Function                                                                                 | Description           | Key Parameters                                  | Usage                              | Documentation                                                              |
| ---------------------------------------------------------------------------------------- | --------------------- | ----------------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------- |
| [createTask(task)](./createTask.js)               | Creates new tasks     | `title`, `description`, `priority`, `project`   | Use BEFORE starting implementation | [Human-Readable Guide](../../../claude_docs/agent-directives/createTask-guide.md) |
| [updateTask(taskId, updateData)](./updateTask.js) | Updates task progress | `status`, `requirements` with 🟢🟡🔴 indicators | Use DURING implementation          | [Human-Readable Guide](../../../claude_docs/agent-directives/updateTask-guide.md) |
| [getTask(taskId)](./getTask.js)                   | Gets a single task    | `taskId`                                        | Use to check task details          | [Human-Readable Guide](../../../claude_docs/agent-directives/getTask-guide.md)    |
| [getTasks(filters)](./getTasks.js)                | Finds matching tasks  | `project`, `status`, `priority`                 | Use to find related tasks          | [Human-Readable Guide](../../../claude_docs/agent-directives/getTasks-guide.md)   |

## Mandatory Import Method

Always import these functions directly from the central client:

```javascript
// Copy this exact import statement
const {
  createTask,
  updateTask,
  getTask,
  getTasks,
} = require('../ixcoach-api/utils/agentTaskClient');
```

## Testing & Documentation Coverage

To verify that all agent functions are properly documented and discoverable:

```bash
# Set up the testing environment
npm run agent-test-setup

# Run the agent documentation test suite
npm run agent-tests
```

These tests simulate Claude's behavior when searching for function documentation and provide metrics on documentation quality and discoverability.

## Mandatory Agent Requirements

As a Claude agent working in this environment, you MUST:

1. **Document ALL work** in the central MongoDB task tracking system BEFORE starting implementation
2. **Create task entries** for each initiative following the MongoDB task documentation template
3. **Log all assignments** using the MongoDB API (not the deprecated .ix-sync files)
4. **Write detailed explanations** of what you did, why, and how to verify it works
5. **Create specific instructions** for the user to check your work
6. **Always note what's next** to be done after your current task
7. **Use branch-based workflow** with separate branches for different concerns
8. **Maintain working state** with each change - ensure each commit leaves the application in a working state

## Before Using These Functions

Before utilizing any of these functions, review the following protocols:

1. **⚠️ MANDATORY: Self-Check Protocol**: Execute the self-check protocol documented in `/Users/jedi/react_projects/ix/claude_docs/CLAUDE_SELF_CHECK.md`. This protocol is MANDATORY and ensures compliance with all IX project directives. No task management should begin without completing this step.

2. **Task Documentation Requirements**: Read the Task Documentation section in `/Users/jedi/react_projects/ix/CLAUDE.md`

3. **Testing Status Indicators**: Always use the standardized indicators (🟢, 🟡, 🔴) when updating tasks

## Workflow Overview

The standard agent workflow for task management follows these steps:

1. **Create Task**: Before starting implementation, document your planned work using `createTask()`
   - Include detailed user impact, requirements, and technical plan
   - Set status as "proposed"
   - Reference: [createTask.js](./createTask.js)

2. **Begin Implementation**: Update the task status to "in-progress" using `updateTask()`
   - Add initial files that will be modified
   - Reference: [updateTask.js](./updateTask.js)

3. **Report Progress**: Regularly update task with progress indicators
   - Mark requirements with 🟢, 🟡, 🔴 to show status
   - Add verification steps as they become clear
   - Reference: [updateTask.js](./updateTask.js)

4. **Complete Task**: Set status to "for-review" when ready for human review
   - Include comprehensive verification steps
   - Document next steps for future work
   - NEVER set status to "done" - tasks require human review
   - Reference: [updateTask.js](./updateTask.js)

5. **Reference Related Tasks**: Use `getTasks()` and `getTask()` to find and reference related work
   - Reference: [getTasks.js](./getTasks.js) and [getTask.js](./getTask.js)

## Implementation Notes

- Always import functions directly from `../ixcoach-api/utils/agentTaskClient.js`
- Follow Test-Driven Development practices and document test results in task updates
- Create separate branches for distinct concerns following the branch naming convention in CLAUDE.md

### Task Linking Requirements

When referring to tasks in the MongoDB system:

1. **Always include a direct link** to the task in the Tasks UI
2. **Use the format**: `http://localhost:3020/task/{taskId}`
3. **Check for production URL** configuration first:
   ```javascript
   // Production task URL takes precedence if defined
   const TASK_UI_URL = typeof PRODUCTION_TASK_UI_URL !== 'undefined' 
     ? PRODUCTION_TASK_UI_URL  // e.g., "https://tasks.ixcoach.com/task/"
     : "http://localhost:3020/task/";
   
   // Usage: ${TASK_UI_URL}${taskId}
   ```
4. **Include task ID and link** in all commit messages and documentation
5. **Add task links** to verification instructions and implementation summaries
6. **Verify links are valid** before including them in documentation

## Example Task Lifecycle

```javascript
// 1. Create new task
const { createTask, updateTask } = require('../ixcoach-api/utils/agentTaskClient');

const newTask = await createTask({
  title: "Add task URL support in task detail view",
  description: "Enable clickable task URLs in the task detail view for easier cross-referencing",
  userImpact: "Users will be able to navigate between related tasks directly from the detail view",
  // ...other fields
  status: "proposed",
  priority: "medium",
  project: "tasks"
});

// 2. Update to in-progress
await updateTask(newTask.id, {
  status: "in-progress",
  branch: "feature/task-detail-url-support"
});

// 3. Update with progress
await updateTask(newTask.id, {
  requirements: 
    "🟢 - URL parsing implemented\n" +
    "🟡 - Click handling implemented but needs testing\n" +
    "🔴 - URL validation not implemented"
});

// 4. Complete task
await updateTask(newTask.id, {
  status: "for-review",
  requirements: 
    "🟢 - URL parsing implemented\n" +
    "🟢 - Click handling implemented and tested\n" +
    "🟢 - URL validation implemented",
  verificationSteps: [
    "1. Start tasks app with `npm run dev`",
    "2. Open a task with references to other tasks",
    "3. Verify URLs are clickable and open the correct task",
    "4. Try invalid URL formats and verify error handling"
  ]
});
```

## Related Documentation

- [Main CLAUDE.md Documentation](/Users/jedi/react_projects/ix/CLAUDE.md) - CRITICAL: Review this file first
- [Self-Check Protocol](/Users/jedi/react_projects/ix/claude_docs/CLAUDE_SELF_CHECK.md) - MANDATORY before starting
- [Quick Reference](/Users/jedi/react_projects/ix/claude_docs/REMINDER.md) - Compact reminder of protocols

### Important Sections in CLAUDE.md

Ensure you review these critical sections in the main CLAUDE.md file:

1. [Task Documentation Template](../../CLAUDE.md#task-documentation-template) - Complete template for task creation
2. [Testing Status Indicators](../../CLAUDE.md#testing-status-indicators) - Guidelines for status reporting
3. [Task Linking](../../CLAUDE.md#task-linking) - Requirements for task URL formatting
4. [Git Commit Protocol](../../CLAUDE.md#git-commit-protocol) - Required formatting for commits

No implementation should begin without reviewing these documents in full.
