# RSI Self Improvement

> v1.1.11
> 2026-11

RSI tests and revises a system prompt or Skill against a set of tasks. Review the original answers, round results, and evaluation evidence, then choose a version to keep.

Open **Self Improvement** in the left navigation. The workflow is **Configure → Iterate → Compare → Export**.

## Choose what to improve

| Improve | Use it for | Task mode |
|---|---|---|
| **Prompt** | Answer structure, wording, decision rules, or task instructions | **Chat** or **Agent** |
| **Skill** | Skill instructions and supporting text files | **Agent** |
| **Both** | A system prompt and Skill tested together as one solution | **Agent** |

The **Teacher Model** defines criteria, proposes revisions, and evaluates results. The **Student Model**, or student agent, performs the tasks. Both roles can use the local models or online connections available in the interface.

RSI saves prompts and Skill files. To update model weights, use [Model Training](../training/overview.md).

## Run your first improvement

1. Click **New Task** (the plus icon) at the top right.
2. Set **Improve** to **Prompt** and **Task mode** to **Chat**.
3. Select the teacher and student models. Enter a **Task Name**, **Improvement Goal**, and **System Prompt**.
4. Under **Evaluation Tasks → Custom Task**, enter a representative question and click **Add Task**.
5. Review the stopping rules and click **Start** (the play icon) at the top right.
6. Watch progress in **Iterate**. When the run ends, open **Compare** and check the answers and evaluation evidence.
7. Open **Export**, check the selected round and its content, then copy it or export the files.

See [Configure and Start a Run](./configure.md) for the full setup. You can also [import evaluation tasks from conversations](./evaluation-tasks.md).

## Before you run

- Only one RSI run can be active at a time. Task count, round count, model speed, and tool execution all affect duration.
- Online models receive the content included in their requests and may incur provider charges. Multiple rounds make multiple model calls.
- Agent tasks use the tools you allow. They may read or change files or access external services. Check the workspace, tool permissions, and auto-approval settings first. Use test data for tasks that could change anything.
- Model-generated scores describe these tasks and this evaluation. Before adopting a result, check its answers, files, and important facts, then try tasks that were not used during improvement.

## Guides

- [Configure and Start a Run](./configure.md): models, goals, parameters, and stopping rules.
- [Evaluation Tasks and Conversation Imports](./evaluation-tasks.md): custom tasks, attachments, history, and missing files.
- [Improve a Skill](./skills.md): select or create a Skill, configure the agent, and edit supported files.
- [Compare and Export Results](./results.md): rounds, evidence, edits, and file saving.
- [History, Continuation, and Troubleshooting](./history.md): retries, continued improvement, notes, and deletion.
