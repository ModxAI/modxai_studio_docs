# Scheduled Tasks

> v1.1.0
> 2026-05

Scheduled tasks allow you to preset automatic execution plans for **agent tasks**, **memory fine-tuning**, or **knowledge base updates**. Tasks can be automatically triggered at fixed intervals or specified times without manual supervision.

## Table of Contents

- [Task Types](#task-types)
- [Scheduling Strategies](#scheduling-strategies)
- [Creating & Editing Tasks](#creating--editing-tasks)
- [Task List & Management](#task-list--management)
- [Run History](#run-history)
- [Task Execution & Stopping](#task-execution--stopping)
- [Notes](#notes)

---

## Task Types

Scheduled tasks support three types:

| Type | Description | Use Cases |
|------|-------------|-----------|
| **Agent Task** | Sends preset prompts to the agent on schedule and executes automatically | Periodic report generation, data scraping, routine inspections |
| **Memory Fine-Tuning** | Automatically exports memories and triggers model fine-tuning | Periodically optimize personal models based on new memories |
| **Knowledge Base Update** | Automatically vectorizes memory data and updates the RAG knowledge base | Periodic data accumulation |

### Agent Task

Automatically triggers the agent to execute specified tasks on a schedule. Configuration options:

| Item | Required | Description |
|------|----------|-------------|
| User Prompt | ✅ | Task instructions to send to the agent |
| Target Session | ✅ | **New Session**: Create a new session each execution / **Existing Session**: Send a message to an existing session |
| Main Model (optional) | ❌ | Temporarily replace the main role model without overwriting the original session configuration |

- **New Session Mode**: Creates a brand new session each time, avoiding context interference.
- **Existing Session Mode**: Appends messages to the specified session, suitable for ongoing conversations. Select from the dropdown if chosen.

> **Note**: When an existing session is selected, if the session is currently running an agent task, the new execution will be blocked until the previous task completes.

### Memory Fine-Tuning

Automatically executes memory fine-tuning on a schedule.

| Item | Required | Description |
|------|----------|-------------|
| Base Model | ✅ | HF Standard base model for fine-tuning |
| Training Template | ❌ | Select a training template preset |
| Memory Category | ✅ | Data range: All Memories / Agent Memories / Bookmarked Memories |
| Minimum Sample Count | ✅ | Minimum memory samples required to trigger training (default minimum 10) |
| Iteration Strategy | ✅ | **Full Rebuild**: Train from scratch each time / **Incremental Iteration**: Continue training from previous results |
| Output Directory | ✅ | Directory for storing training results |

> **Prerequisites**: No other running agent or training tasks should be in memory.

### Knowledge Base Update

Automatically vectorizes memory data and writes it to a RAG knowledge base on a schedule.

| Item | Required | Description |
|------|----------|-------------|
| Embedding Model | ✅ | Embedding model for generating vectors; must be imported in the model library first |
| Output Directory | ✅ | Knowledge base data output directory |
| Knowledge Base Name | ❌ | Default `modxai_rag_memory`. A fixed name ensures multiple updates accumulate in the same knowledge base |
| Vector Normalization | ❌ | Whether to normalize vectors; enabled by default |
| Target Dimension | ❌ | Target vector dimension; default 1024 |
| Pooling Strategy | ❌ | Vector pooling method: `mean` / `max` / `cls`; default `mean` |

> **Prerequisites**: If no embedding model is available, import an Embedding type model in the model library first.

---

## Scheduling Strategies

Scheduled tasks support two trigger methods:

### Interval Trigger

Repeats execution at a fixed time interval. Configuration:

| Parameter | Description | Options |
|-----------|-------------|---------|
| Interval Value | Positive integer | e.g., 30 |
| Interval Unit | Time unit | Minutes / Hours / Days / Weeks / Months / Years |

Example: "Every 30 minutes" or "Every 1 day."

### Fixed Time

Executes once at a specified time point. Select the date and time. The execution time must be later than the current time.

---

## Creating & Editing Tasks

### Creating a Task

1. Click the **New Task** button.
2. Enter a task name (required).
3. Select the task type from the dropdown.
4. Configure the scheduling strategy (interval or fixed time).
5. Fill in task-specific configuration based on the task type.
6. Click Confirm to create.

### Editing a Task

Find the target task in the task list and click **Edit**. Modify parameters and save.

- If the scheduling strategy parameters are modified, the system automatically recalculates the next execution time.

### Enable/Disable

Use the toggle switch in the task list to enable or disable a task at any time. Disabled tasks do not trigger automatically but can still be triggered manually.

---

## Task List & Management

The task list displays all created scheduled tasks:

| Column | Description |
|--------|-------------|
| Name | Task name |
| Task Type | Agent Task / Memory Fine-Tuning / Knowledge Base Update |
| Trigger Method | Displays "Every X minutes" or a fixed time point |
| Next Execution | Estimated next execution time |
| Last Status | Status of the most recent execution |
| Enabled | Toggle switch |

Click on a task row to select it; its run history expands below.

---

## Run History

After selecting a task, its recent execution records are displayed below.

### Run Status

| Status | Description |
|--------|-------------|
| Queued | Task created, waiting for execution |
| Running | Task is executing |
| Success | Task completed successfully |
| Failed | Task execution error |
| Blocked | Task cannot execute due to resource contention, etc. |
| Stopped | Manually terminated |

### Trigger Source

| Source | Description |
|--------|-------------|
| Scheduled Trigger | Automatically triggered on schedule |
| Manual Trigger | Triggered via the "Execute Now" button |
| Retry | System automatic retry |

### Operations

- **View Run Details**: Shows status, trigger source, start/end time, and error summary.
- **Stop Execution**: Manually terminate a running task (only available during execution).
- **Delete Record**: Delete the run history entry (not available during execution).
- **Clear Records**: Clear all run history for the task (except currently running entries).

---

## Task Execution & Stopping

### Manual Execution

Click the **Execute Now** button on a task row to manually trigger execution, regardless of the scheduling strategy. Results are recorded in the run history.

### Stop Execution

If a task is running (status "Running" or "Executing"), click **Stop Current Task** to terminate it.

### Concurrency Limitation

> **Note**: Scheduled tasks use a global single-concurrency policy — only one task can execute at a time. If a new task's trigger time arrives while a previous task is still running, the new task will be blocked or skipped.

---

## Notes

- Memory fine-tuning export uses the export capabilities from "Memory Management"; compatibility rules are the same as manual export.
- Knowledge base updates use `modxai_rag_memory` as the default collection name; multiple executions will **cumulatively update** the same knowledge base.
- The scheduler scans for due tasks every 15 seconds.
- If the ModxAI main service restarts, the scheduler automatically restores the status of incomplete tasks.
- In "Existing Session" mode for agent tasks, execution will fail if the target session has been deleted.
