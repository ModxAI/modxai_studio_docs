# History, Continuation, and Troubleshooting

> v1.1.11
> 2026-11

Use History to revisit results, prepare another improvement run, or deal with stopped and failed runs.

## Find a run

1. Open **Self Improvement** and click **History** (the clock icon) at the top right.
2. Filter by **Improve**, or use **Search Titles** to find a task name.
3. Open a record to review its status, rounds, and results. Click **Load More** for additional records.

You can view other records while a run is active. The top Stop button applies to the active run; hover over it to check the task name. Use **View Active Run** to return to it.

## Continue improving or start fresh

**Continue Improving** prepares a new run from an existing result:

1. Open a historical run with an available result and click **Continue Improving**. For a failed run, this may be under **More Actions**.
2. Review the imported goal, models, tasks, prompt, and Skill. Adjust them if needed.
3. Click **Start** to create a new run. The original record is retained.

Continuation uses the system's best solution when one is available, or an existing candidate when no valid best solution exists. It does not use a different round merely because you selected it in Compare, or automatically include manual edits made in Export. Check the new configuration and add any changes you need.

Continuation is unavailable if only the initial solution exists, no usable candidate is available, or another RSI run is active. For a blank setup, click **New Task**. This clears the current configuration draft without deleting previous runs.

## Review improvement notes

In **Compare → Improvement Notes**, read lessons from this run and lessons used from earlier runs. Notes include observations, applicable conditions, and ineffective attempts. Use **View Source** on a referenced note to open its original record.

A **Completed** run is marked **Adopted** after its unedited **Best Solution** is successfully exported to a directory. If the baseline and at least one candidate have valid comparisons, improvement notes are then generated.

- Copying text, exporting another round, or saving manually edited content does not mark the best solution as adopted.
- A note-generation failure does not undo saved files. Click **Retry** in the notes section, or **Summarize Notes** when that action is available.
- Subsequent runs created with **Continue Improving** can use notes from adopted results in the same series. A blank New Task does not automatically inherit that series.
- Notes summarize model-observed results. Check whether each lesson fits the current task before relying on it.

## Stops, failures, and retries

| Status or situation | What to do |
|---|---|
| **Stopping** | Wait for it to finish before starting another run. Stopping does not undo completed actions. |
| **Failed**, with an available **Retry** button | Read **Errors**, resolve problems such as connectivity, and retry. Retry uses the original configuration and continues unfinished work while retaining saved valid results. |
| An agent accepted the task but did not finish | Use **View Conversation** to inspect the original conversation, approvals, input, and execution state before deciding to start a new run. Avoid repeating actions that may already have changed something. |
| **Interrupted** | A run does not automatically resume after an application restart. Existing results remain available for review, continuation, or a new task. |
| A fix requires different settings | Use **Continue Improving** or **New Task**. Retry does not apply a new model or new parameters. |

## Troubleshooting

### Start is unavailable

Check the task name, improvement goal, models, and added tasks. Supply missing files. Agent mode needs a complete configuration; Skill mode needs a valid Skill. Confirm permission to share tool records and file content if using an online teacher.

### The context is too small

Check whether the error concerns the teacher or student. In a new or continued configuration, increase that model's context, reduce task material, or lower the output limit. Retry only continues the original unfinished work; it does not increase capacity.

### An online model or connection is unavailable

Check its connection and credential in Settings, then refresh the model list. If the connected model or configuration changed, select the model again and create a new run.

### A result cannot be compared

Read the specific warning and inspect the linked conversation. Content may still be available when a Skill was not loaded, execution did not complete, or evidence was insufficient, but it does not establish a valid improvement. Resolve the cause and test a new solution.

## Delete records

1. Click **Select** in **History**.
2. Select records, or use **Select all** for the deletable records currently loaded.
3. Click **Delete selected runs**, check the count, and confirm.

You can also use a record's individual delete button. Records may be unavailable for deletion while execution, stopping, note generation, or linked agent activity is still in progress.

A deleted record cannot be restored from RSI History. Save any results you need first. Exported files and linked agent conversations are not deleted with the RSI record. Manage those conversations separately in the Agent page.

Related: [Configure and Start a Run](./configure.md) · [Compare and Export Results](./results.md)
