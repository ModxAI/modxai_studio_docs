# Agent Workbench

> v1.1.0
> 2026-05

The Agent Workbench is the core functional module of ModxAI Studio. Unlike standard chat, the agent **autonomously decomposes tasks, selects tools, executes steps, and produces final results** — you simply describe the goal, and the agent handles web search, file reading, command execution, report generation, and more automatically.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Core Concepts](#core-concepts)
- [Session Management](#session-management)
- [Sending Messages](#sending-messages)
- [Execution Process](#execution-process)
- [Notes](#notes)
- [More Features](#more-features)

---

## Prerequisites

The Agent Workbench has **minimum configuration requirements** for models and environment. Without meeting these conditions, it will not function properly.

### Model Requirements

> **A model must be configured for the "Main Role"**, otherwise new sessions cannot be created or messages sent.

| Item | Minimum | Recommended |
|------|---------|-------------|
| Model Family | Qwen3 / Qwen3.5 or newer | Qwen3-8B or above |
| Parameter Size | ≥ 4B | 8B~32B |
| Context Length | ≥ 32K tokens | 64K~128K |
| Quantization | Q4_K_M or higher | Q5_K_M / Q6_K |

> **Why these requirements?** The agent must simultaneously understand task objectives, break down steps, read tool outputs, and make decisions. Smaller models or those with shorter context windows tend to get stuck in loops or fail to complete multi-step tasks.

Model sources:
- **Local Models**: Download and load from the model library (supports LLM / VLM / HF standard models)
- **Online Models**: Connect to cloud providers via API Key (supports DeepSeek, OpenRouter, etc.)
- **LAN Models**: Connect to model services within the local network

### Browser & Web Search Environment

The agent's **browser operations** and **web search** features depend on additional Python libraries. To use these tools, install the agent environment dependencies in **Settings -> Environment Configuration**.

- Browser and search tools are unavailable if the related dependencies are not installed.
- Basic features like file read/write and terminal commands require no additional installation.

### Voice Input Environment

Voice input requires loading **STT (Speech-to-Text)** and **TTS (Text-to-Speech)** models. See [Voice Input](./voice-input.md).

---

## Quick Start

### Step 1: Configure the Main Role Model

1. Open the Agent Workbench page
2. In the right **Session Configuration** panel, expand "Role Parameters" -> "Main Role"
3. Select the connection type (Local / Online / LAN) and specify the model
4. The agent is ready for use once the model is selected

> When no model is configured for the main role, the **New Session** button and **Send** button are disabled, and the interface prompts "Main role has no model configured; cannot create a new session."

### Step 2: Create a Session

Click **New Session** on the left. A temporary title (e.g., "New Session-10:30") is automatically generated, and it updates to a formal title after the first task is sent.

### Step 3: Describe the Task

Describe your goal in natural language in the input box. For example:
- "Search the web for the latest AI developments and compile a summary report."
- "Read the README.md in my workspace, extract the core content, and polish it into an email."
- "Analyze the data.csv file and generate a statistical report."

Press `Enter` to send, and `Shift + Enter` for a new line.

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

Each role's model and parameters can be independently configured. Except for the main role, if a role has no model specified, it automatically reuses the main role's model. See [Role Configuration](./roles.md).

### Toolset

The agent can invoke various tools to complete your tasks:

| Tool Category | Capabilities |
|---------------|--------------|
| **File Operations** | Read, write, move, search files |
| **Terminal Execution** | Run commands and scripts |
| **Web Search** | Search engine queries, web page content extraction |
| **Browser** | Launch browser, navigate pages, take screenshots, interact |
| **Crawler** | Batch scrape website content |
| **Todo Planning** | Create and update task todo lists |
| **Delegate Collaboration** | Assign subtasks to other roles |
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

Click **New Session** on the left. A temporary title is auto-generated, and it updates to a formal title based on the task content after the first message is sent.

### Switch Session

Click any session in the left session list to switch. The session's message history and execution progress are restored automatically.

### Rename Session

Click the menu on the right side of a session, select **Rename**, enter a new title, and save.

### Delete Session

Click the menu on the right side of a session, select **Delete**. Deletion is irreversible.

### Archive Session

For sessions you want to keep but don't need to see immediately, use the **Archive** feature. Archived sessions are hidden from the main list and can be viewed and restored via **Archive Management**. Single and batch operations are supported.

---

## Sending Messages

### Basic Send

Enter your task description in the input box and press `Enter`. The agent will automatically begin analysis and execution.

### Send Modes

- **Send to New Session**: Start a new round of conversation in the current session, sending the message as a new task.
- **Append to Current Session**: Append the message to the current task; the agent continues execution based on the existing context.

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

Performing a **Resend** operation deletes the current message and all subsequent content, then resends it.

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
