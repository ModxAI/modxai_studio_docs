# Configure and Start a Run

> v1.1.11
> 2026-11

Choose models and an improvement goal, then test each version against the same set of tasks.

## Prerequisites

- For local models, download or import a model in [Model Library](../models/overview.md) and prepare its required runtime environment.
- For online models, add a working connection and credential in Settings first.
- Prepare at least one evaluation task. Select **Agent** mode for tasks that need tools, regular files, or audio input.

## Configure a task

1. Open **Self Improvement** and click **New Task**.
2. Choose what to **Improve**. Start with **Prompt**, or see [Improve a Skill](./skills.md) for Skill and combined runs.
3. Under **Guide & Evaluate**, select a **Teacher Model**. Choose a local model from the list, or select an online credential and then a model. Click **Refresh** if the list is outdated.
4. Under **Execute & Test**, choose the **Task mode**:
   - **Chat**: select the student model and parameters directly. Use this for tasks such as questions that need no tools.
   - **Agent**: click **Configure** (the gear icon) and review the main-role model, tools, workspace, Skills, and approvals. These settings apply to the agent used by this RSI run.
5. Enter a **Task Name** so you can find the run in History.
6. Enter a specific **Improvement Goal**, such as: “State the conclusion first, then the steps. If the source does not specify a date or amount, say it is unknown.”
7. Enter the **System Prompt** to improve. You can expand **Quick fill** and edit an example. In Chat mode, leaving this blank uses the default system prompt.
8. [Add or import evaluation tasks](./evaluation-tasks.md) and check that they appear in the list.
9. Review **Stopping Rules** and click **Start** at the top right.

An agent run with an online teacher requires **Share tool records and file content with the online model for grading**. Check that the task material can be sent to the selected service. The student agent's own online models and external tools may also transmit content they process.

## Set stopping rules

| Setting | Meaning |
|---|---|
| **Max Rounds** | 1–10, with a default of 3. A baseline runs first, so the maximum is the baseline plus the configured number of improvement rounds. |
| **Target Score** | 0–100. Stops when the best round's average task score reaches the target. The baseline can trigger it too. Leave blank to disable. |
| **Minimum Score** | 0–100. Stops if an improvement round's average task score falls below this value. The baseline is excluded. Leave blank to disable. |

These are stopping conditions, not guarantees of correctness. For a first run, use a small number of rounds and check whether the tasks and criteria are appropriate.

## Adjust model parameters

After selecting a model, expand **Model Parameters** if needed. Available options depend on the model type.

| Parameter | Guidance |
|---|---|
| **Context Length** | Leave blank to use the model settings; 0 uses the model's context limit. A small context may not fit the tasks, material, and evaluation evidence. |
| **Maximum Output Tokens** | 0 uses an automatic output budget. Increase it if needed, while leaving room for the input. |
| **Temperature**, **Top P**, **Top K**, **Min P**, **Repetition Penalty** | Control generation. Start with the defaults and adjust for your task. Changing these also changes the comparison conditions. |
| **Thinking Depth** | Choose an option supported by the model. Deeper reasoning may take longer and increase online charges. |
| **LoRA** | Select an existing adapter for a compatible local model. |
| **Device**, **Precision**, **Model Task** | Available for applicable model types. Choose settings that fit the model and your environment. |

## Watch progress

The run opens in **Iterate**, showing status, elapsed time, and stages such as Define Criteria, Run Baseline, Evaluate Baseline, and Iterate. Expand **Criteria** to inspect the requirements. Use **Rounds** to read each task's answer in each round.

If an agent needs approval or input, click **View Conversation**, respond in the linked conversation, and return to RSI. Do not start another task just because a stage is taking time.

To end a run, click **Stop** and wait for **Stopped**. You cannot start another RSI run while it is stopping. Stopping does not undo file changes or external actions that already completed.

A started run's configuration is read-only. To change models, goals, tasks, or round limits, use **Continue Improving** or **New Task**. See [History and Troubleshooting](./history.md).
