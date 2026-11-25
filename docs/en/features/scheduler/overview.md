# Scheduled Tasks

Run an agent, curate memories, process data, train a model, or run self-improvement on a schedule. You can also pass the output from one successful task to another.

In the sidebar, open **More → Tasks**.

## Task Types

| Type | What to prepare |
| --- | --- |
| **Agent** | Prepare a main-role model, tools, and task prompt. If using an existing conversation, check its settings first. |
| **Memory Curation** | Use a local main model. Choose the last 24 hours, selected conversations, or a date range. |
| **Data Processing** | Prepare the files, models, and environment for the selected data type, such as generating training samples from text or updating a knowledge base. |
| **Model Training** | Prepare the training environment and a template. Choose training data or bind a training file from an upstream task. This category also includes memory fine-tuning. |
| **Self-Improvement** | Prepare the prompt or Skill configuration, evaluation tasks, and models. |

## Create and Edit Tasks

1. Select **New Task**, enter a name, and choose a type.
2. Specify the inputs and choose a **Preset** or training template, if needed. Select the gear to open task settings. **This task only** leaves the original preset unchanged. To reuse the settings elsewhere, select **Save as preset** or **Save as template**.
3. Under **Schedule**, choose the trigger. For dependencies, select an **Upstream Source**. If using a timed rule, check the date, weekdays, and time.
4. Save, then check whether the task is enabled and when it runs next. To check the output first, run the task once from the list and review its run record.

When editing an existing task, recheck its inputs and trigger. Disabling a task prevents future triggers. To end an active run, stop it in its run record.

## Scheduling Strategy

Run tasks at an interval or a specified time. To wait for another scheduled task to produce output, select it as the upstream task. Choose **Upstream Only** when the task should run only from that trigger.

### Upstream Task

For example, prepare training data first, then train a model:

1. Save a **Data Processing** task that produces training data.
2. Create a **Model Training** task. Under **Upstream Source**, choose **Scheduled Task** and select the task you just saved. Select **Upstream Only** and run only on upstream success.
3. Select **Bind artifact** and map **Training data** to **Training file**. If available, bind matching validation data to **Validation file**. Check the template and data format, then save and enable the two tasks.
4. After the first task finishes, check the downstream **Run History** for its inputs and results. If a required output is missing, review the upstream run before retrying.

To use the output of an already completed run, use **Select Run** for the appropriate source. This is different from waiting for the next upstream trigger. The picker shows outputs that are compatible with the selected target.

## Run History

Select a task to review run status, triggers, errors, and result files. Select **Open** on an available result, or return to the run from the [Inbox](../agent/inbox.md).

## Run and Stop Tasks

- Run a task once from the list without changing its schedule.
- Stop the active run from its run record. Future scheduled runs remain enabled. To pause the schedule, disable the task.
- Scheduled tasks do not start in parallel. If another task is running, check the run history and the next trigger time.

## Notes

Keep the app open and the device running for scheduled execution. Check that models, files, and permissions will still be available. Tasks using online models or external tools need a connection and may incur provider charges. Before enabling unattended runs, review tools and approvals, especially for tasks that modify files, start training, or call an external service.
