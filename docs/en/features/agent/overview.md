# Agent Workbench

> v1.1.11
> 2026-11

Describe the goal in the agent workbench. The agent splits the steps, calls tools, and shows progress. Tools include web search, file reads, commands, and writing a result.

Hover over the corresponding conversation entry in the main menu to view its conversation list. Selecting a conversation opens it. Use **Runtime Settings** at the top right to adjust the current conversation.

Click the page title to rename the current conversation. The hover list is only for selecting conversations. To rename or delete from the list, pin the submenu and use the pencil or trash button beside the conversation.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Core Concepts](#core-concepts)
- [Session Management](#session-management)
- [Sending Messages](#sending-messages)
- [Execution Process](#execution-process)
- [Notes](#important-notes)
- [More Features](#more-features)

---

## Prerequisites

Choose a model for the main role, then install the tools your tasks need.

### Model Requirements

> **Configure a model for the Main Role** before creating a session or sending a message.

Prefer a model with good instruction-following and tool-calling capabilities. Parameter count alone does not determine whether a task will succeed.

- **Local models**: Choose a model and quantization level that fit your RAM and VRAM. Check the results with a simple task first.
- **Long tasks**: A 32K context is a starting recommendation. Adjust it within the model’s supported range and your available resources; it is not a universal minimum.
- **Check results**: Models can miss steps or give incorrect answers. Review important actions and final results.

Model sources:
- **Local Models**: Download and load from the model library (supports LLM / VLM / HF standard models)
- **Online Models**: Connect to cloud providers via API Key (supports DeepSeek, OpenRouter, etc.)
- **LAN Models**: Connect to model services within the local network

### Browser & Web Search Environment

The agent's **browser operations** and **web search** features depend on additional Python libraries. To use these tools, install the agent environment dependencies in **Settings → System → Environment Configuration**.

- Browser and search tools are unavailable if the related dependencies are not installed.
- Basic features like file read/write and terminal commands require no additional installation.

### Voice Input Environment

Voice input requires a microphone and a loaded **STT (Speech-to-Text)** model. Load a **TTS (Text-to-Speech)** model only if you want replies read aloud. See [Voice Input](./voice-input.md).

---

## Quick Start

### Step 1: Configure the Main Role Model

1. Open the Agent Workbench page
2. Click **Runtime Settings** (gear icon) at the top right, then expand "Role Parameters" → "Main Role"
3. Select the connection type (Local / Online / LAN) and specify the model
4. The agent is ready for use once the model is selected

> When no model is configured for the main role, the **New Conversation** button and **Send** button are disabled, and the interface prompts "Main role has no model configured; cannot create a new session."

### Step 2: Create a Session

Click **New Conversation** in the conversation list, or use the plus button at the top of the page. A temporary title (e.g., "New Session-10:30") is automatically generated, and it updates to a formal title after the first task is sent.

### Step 3: Describe the Task

Describe your goal in natural language in the input box. For example:
- "Search the web for the latest AI developments and compile a summary report."
- "Read the README.md in my workspace, extract the core content, and polish it into an email."
- "Analyze the data.csv file and generate a statistical report."

Click Send or use the shortcut selected in **Settings → General → Editor**. Press `Shift + Enter` for a new line.

---

## Core Concepts

### Role System

The agent uses a **multi-role collaboration** architecture, where each role has distinct responsibilities:

| Role | Description | Required |
|------|-------------|----------|
| **Main Role** | Handles primary reasoning, tool orchestration, and final response | ✅ Required |
| **Evaluate Role** | Reviews response quality, provides feedback and revision suggestions | Enabled by default, optional |
| **Summary Role** | Context compression and auxiliary summarization | Optional |
| **Memory Role** | Fuzzy memory search via fine-tuned model | Optional |
| **Security Role** | Pre-screening of high-risk content and safety recommendations | Optional |
| **Delegate Role** | Delegates subtasks to independently configured roles | Optional |

Each role can have its own model and parameters. Summary, Security, Evaluate, and Delegate roles reuse the main model when no model is specified. The Memory role requires a local model. See [Role Configuration](./roles.md).

### Toolset

The agent can invoke various tools to complete your tasks:

| Tool Category | Capabilities |
|---------------|--------------|
| **File Operations** | Read, write, move, search files |
| **Load external image** | Sends an external image to a multimodal model. Without the multimodal projection component, the model handles text only. Images in a read-only external workspace can be read too |
| **Terminal Execution** | Run commands and scripts |
| **Web Search** | Search engine queries, web page content extraction |
| **Browser** | Launch browser, navigate pages, take screenshots, interact |
| **Crawler** | Batch scrape website content |
| **Todo Planning** | Create and update task todo lists |
| **Delegate** | The main role hands a subtask to another configured role. Call it by role name. This is not a separate multi-agent product |
| **Memory Management** | Search, read, save long-term memory and runtime reminders |
| **Interaction** | Ask user questions and make suggestions |
| **Computer** | Screen capture |
| **Multimedia** | Image generation, video generation, speech synthesis |

The allowed tools for each role can be selected in the right panel's "Toolset."

### Advanced Parameters

Advanced parameters allow fine-tuning of agent behavior, such as tool execution timeout, web search limits, browser timeout, and security policies. See [Advanced Settings](./advanced-settings.md).

---

## Session Management

### New Session

Click **New Conversation** in the conversation list, or use the plus button at the top of the page. A temporary title is auto-generated, and it updates to a formal title based on the task content after the first message is sent.

### Switch Session

Click any session in the left session list to switch. The session's message history and execution progress are restored automatically.

### Rename Session

Pin the submenu, click the pencil beside a conversation, enter a title, then press `Enter` or click the check mark. You can also click the page title to rename the current conversation.

### Delete Session

Pin the submenu, click the trash button beside a conversation, and confirm. Deletion is irreversible.

### Archive Session

For sessions you want to keep but don't need to see immediately, use the **Archive** feature. Archived sessions are hidden from the main list and can be viewed and restored via **Archive Management**. Single and batch operations are supported.

---

## Sending Messages

### Basic Send

Enter your task in the input box, then click Send or use your configured send shortcut. Drag the edge of the input box to adjust its height.

Long pasted text in the input box can be collapsed. Expand it to read the full text.

### Send Modes

Choose the default **Message Mode** under **Settings → Agent → Messages**:

- **Queue**: Send the message after the current turn ends. Use this for the next task.
- **Steer**: Add guidance to the current task, to be handled when the agent can accept another message.

During a run, `Alt + Enter` sends only this message in the opposite mode without changing the default. When no task is running, the message starts a new turn.

### Manage Queued Messages

Expand **Queued Messages** above the input box:

- **Send Now**: Submit the selected message.
- **Edit**: Change a queued message that has not been sent. Saving keeps it in the queue.
- **Delete**: Remove a message you no longer need.

Messages being delivered cannot be edited. If delivery needs review, check the conversation result before sending again. Guidance that the current turn did not use remains available to send or delete.

## Message Operations

### Edit Message

Sent user messages can be edited:

1. Click the **Edit** button on the right side of the message.
2. Modify the message content.
3. Save to update.

### Delete Message

Two deletion scopes are supported:

- **This Message Only**: Deletes only the current message.
- **This and Subsequent**: Deletes the current message and all messages after it.

> Messages already processed by a running agent cannot be deleted.

### Resend Message

**Resend** asks for a second confirmation. After you confirm, it deletes that message and everything after it, then sends the message again.

### Copy Message

Click the **Copy** button to copy the message content to the clipboard.

### Memory Feedback

The following feedback options are available for agent responses:

- **Preferred Memory** (👍): Marks the response as satisfactory memory for future reference.
- **Avoid Memory** (👎): Marks the response as a pattern to avoid.

Feedback is automatically saved as long-term memory, influencing future agent behavior.

---

### Attachments

When sending a message, you can attach the following content, which the agent will automatically read and analyze:

| Attachment Type | Description |
|---------|------|
| **Local Attachments** | Images, audio, video, files |
| **Knowledge Base Connection** | Links to previously created RAG knowledge bases |
| **Workspace Reference** | Reference other workspace directories(Read-only) |

For more details, please refer to [Attachments & Knowledge Base](./attachments.md).

### Prompt Optimization

Helps you quickly generate high-quality prompts to enhance the effectiveness of your AI interactions. For more details, see [Prompt Optimization](../chat/prompt-generation.md).

### Voice Input

Supports voice input via microphone, with automatic speech-to-text transcription. **Requires STT model loading**. For more details, see [Voice Input](./voice-input.md).

---

## Execution Process

### Real-time Progress

While the Agent is performing a task, the interface will display real-time updates:

- **Progress Nodes**: The status and results of each analysis step and tool call.
- **To-do Panel**: A real-time updated checklist of current task items and completion progress.

- **Reasoning Process**: Visible when "Thinking Mode" is enabled, displaying the AI's logical reasoning.
- **Notices**: The area above the input box shows run notices, errors, file changes, to-dos, and queued messages. Expand or collapse it as needed. Use **Restore Notices** to show temporarily hidden run notices, file changes, to-dos, and queued messages again.
- **Duration**: Total task time and the current status stay on screen for the whole run. They do not disappear when a single step refreshes.

### Auto-Approval

When **Auto-Approval** is enabled, the Agent will automatically proceed with operations that require confirmation (such as executing terminal commands) without manual intervention.

> ⚠️ Auto-approval should only be used in scenarios where you trust the Agent. When disabled, high-risk operations will pause and wait for your confirmation. For more details, see [Auto-Approval & Security](./auto-approval.md).

### Pause & Resume

The Agent will pause and wait for your decision in the following scenarios:
- Executing commands that may modify files or system settings.
- Security audits detecting risky content.
- Reaching the maximum number of tool call iterations.
- AI requesting human selection or decision-making.
- The model making no effective progress temporarily.

When paused, you can choose to **Continue** or **Stop**.

### Context Compression

Click the **Context Usage** ring at the top of the page to view usage. For manual compression, click **Compress Context** in the popup. This button is unavailable while the agent is running.

In long conversations, the agent compresses older messages and retains key information before continuing. Some details may be lost, so state important requirements in the current task. See [Advanced Settings](./advanced-settings.md) for related controls.

### Rollback Operation

For files created or modified by the Agent within the workspace, **Rollback** functionality is supported to revert to a previous state. Click the **Rollback** button next to the modification node and confirm to restore.

---

## Important Notes

### Model Configuration

- **The Primary Role model must be configured first** before you can create sessions or send messages.
- The Primary Role supports three connection methods: Local, Online, and LAN; any one of these is sufficient.
- For online models, an API Key must first be added in **Credential Management**.
- The Memory Role strictly requires a local model and does not support Online or LAN connections.
- If no model is configured for Summary/Security/Evaluation/Delegation roles, the Primary Role's model will be used by default.

### Tool Dependencies

- **Browser** and **Web Search** require Agent environment dependencies (installable via Settings). If not installed, only these specific tools will be unavailable; other functions remain unaffected.
- **TTS (Text-to-Speech)** and **STT (Speech-to-Text)** require the corresponding models to be loaded.
- **Multimedia Tools** require their respective models to be loaded; otherwise, effective results cannot be obtained.

### Execution Security

- The Agent can execute terminal commands and modify files; please use it within a trusted workspace.
- For important file changes and terminal commands, turn off **Auto-Approval** and review requests before approving them.
- A **Command Blacklist** can be configured in Advanced Parameters to prohibit the execution of specific commands.
- Enabling the **Security Role** adds an additional layer of security auditing.

### Performance Recommendations

- Choose a model for your task and hardware. See [Model Requirements](#model-requirements).
- When a VRAM warning is shown, GPU tasks run one at a time. An idle GPU is not left stopped. When VRAM is critically low, a new GPU task is refused.

### Workspace

- If no workspace directory is specified, the system uses a default workspace within the temporary directory.
- Agent file operations are performed within the scope of the workspace by default.
- You can customize the workspace directory in the Session Configuration.
- File preview, notes shared across sessions, and links to earlier tasks are covered in the [Workspace Browser](./workspace.md).

---

## More Features

| Feature | Description | Documentation |
|------|------|------|
| Role Configuration | Independent settings for models, parameters, and tools for each role | [Role Configuration](./roles.md) |
| MCP External Tools | Connect services and select role tools | [MCP External Tools](../mcp/overview.md) |
| Advanced Parameters | Fine-grained control over tool timeouts, search limits, security policies, etc. | [Advanced Settings](./advanced-settings.md) |
| Attachments & KB | Usage methods for files, images, knowledge bases, etc. | [Attachments & KB](./attachments.md) |
| Voice Input | Microphone recording and transcription to text | [Voice Input](./voice-input.md) |
| Auto-Approval & Security | Auto-approval toggle, security audit mechanisms, command blacklist | [Auto-Approval & Security](./auto-approval.md) |
| Instruction Settings | Global instruction files and personal preference configurations | [Instruction Settings](./profile-settings.md) |
| Preset Management | Save and load workbench parameter presets | [Preset Management](./presets.md) |
| Workspace Browser | Preview files, share notes across sessions, link earlier tasks | [Workspace Browser](./workspace.md) |
| SD Chat | Image and video generation. Open it from the agent submenu | [SD Chat](../sdchat/overview.md) |
| Speech Synthesis | Text-to-speech. Open it from the agent submenu, next to SD Chat | [TTS](../tts/overview.md) |
