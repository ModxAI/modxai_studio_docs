# Use Studio features from an agent

From a conversation, an agent can query or start data processing, model training, self-improvement, and scheduled tasks. It can also check progress and find results.

## Get started

1. In Agent Workbench, select the wrench in the header to open the main role's tool picker.
2. Open a group such as **Data Processing** or **Scheduled Tasks**. Select read tools if you only want to look something up; enable the relevant action tools if the agent also needs to start or edit a task. Confirm the selection and return to the conversation.
3. Describe the task, the files or preset to use, where to save the output, and whether to start now. For training runs or schedules, ask the agent to show its settings before starting if you want to review them.
4. If the app asks you to authorize a file or confirm an action, review the scope before approving. For background tasks, check the result card in the conversation or the [Inbox](./inbox.md).

Example: “List the available text processing presets. Once I've picked one, check the Markdown files in my workspace and tell me the input and output paths. Don't start processing yet.”

Available actions depend on the main role's enabled tools and access to the resources. Files outside the workspace may need separate authorization. Training and self-improvement still require their own models and environment. An **Accepted** status means the request was received, not that the task finished.
