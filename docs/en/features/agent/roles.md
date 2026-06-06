# Role Configuration

> v1.1.0
> 2026-05

The agent uses a multi-role collaboration architecture, where each role handles different aspects of a task. This topic describes the purpose and configuration of each role.

## Table of Contents

- [Role Overview](#role-overview)
- [Main Role](#main-role)
- [Evaluate Role](#evaluate-role)
- [Summary Role](#summary-role)
- [Memory Role](#memory-role)
- [Security Role](#security-role)
- [Custom Roles](#custom-roles)
- [Common Configuration Items](#common-configuration-items)

---

## Role Overview

| Role | Core Responsibility | Default Status | Tool Permissions |
|------|--------------------|----------------|------------------|
| Main Role | Reasoning, tool orchestration, final response | Required | All optional |
| Evaluate Role | Review response quality, provide feedback | Enabled by default | Limited tools by default |
| Summary Role | Context compression, auxiliary summarization | Disabled by default | Limited tools by default |
| Memory Role | Fuzzy memory search (fine-tuned model) | Disabled by default | Tools prohibited |
| Security Role | High-risk content pre-screening | Disabled by default | Tools prohibited by default |
| Delegate Role | Execute delegated subtasks | Added on demand | Independently configurable |

---

## Main Role

**The main role is the only required role.** Without a configured model for the main role, new sessions cannot be created or messages sent.

### Core Responsibilities

- Understand user task objectives and break them down into execution steps
- Select appropriate tools to complete tasks
- Generate final responses
- Delegate subtasks to other roles as needed

### Connection Types

The main role supports three model connection methods:

| Type | Description | Configuration Notes |
|------|-------------|-------------------|
| **Local** | Use imported local models from the model library | Select model category -> Select specific model |
| **Online** | Connect to cloud providers via API Key | Select provider -> Bind API credentials -> Select model |
| **LAN** | Connect to model services on the local network | Enter service URL |

### Recommended Models

- **Qwen3.5 Series** (9B+): First choice, strong tool-calling and instruction-following capabilities.
- At least 4B parameters, context window recommended at 32K or above.
- Quantization format Q4_K_M or higher recommended.

### Default Inference Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Temperature | 0.6 | Controls response randomness |
| Max Generation Length | 0 (auto) | 0 means auto-calculated as 30% of context |
| Max Tool Rounds | 100 | Maximum tool call rounds per single task |
| Inference Timeout | 600 s | Maximum wait time per inference |
| Streaming Output | Enabled | Real-time content display |
| Thinking Mode | Enabled | Supports model-native thinking blocks |

---

## Evaluate Role

**Enabled by default**, responsible for reviewing the quality of the main role's formal response.

### Core Responsibilities

- Evaluate whether the response meets user requirements
- Check logical completeness and factual accuracy
- Suggest modifications or request regeneration
- Release the response only when quality standards are met

### Configuration Notes

- Enabled by default; can be disabled in role parameters.
- When disabled, the main role's response bypasses quality review.
- Inference parameters are conservative (temperature 0.6) to ensure evaluation consistency.
- Tool rounds limited to 5.
- Streaming output not supported.
- If no model is specified, automatically uses the main role's model.

---

## Summary Role

**Disabled by default**, used for context compression and auxiliary summarization.

### Core Responsibilities

- Compress historical messages when conversation length approaches the model's context window
- Generate context summaries that can be used for continued execution
- Reduce memory and computation overhead of long conversations

### Configuration Notes

- HF / Online / LAN standard models recommended (e.g., small summarization models)
- Default temperature 0.4, conservative to ensure summary quality
- Streaming output not supported
- Recommended for long-task scenarios
- If no model is specified, automatically uses the main role's model

---

## Memory Role

**Disabled by default**, used for fuzzy memory search based on a fine-tuned memory model.

### Core Responsibilities
- Assist the main role with fast fuzzy memory matching.

### Important Constraints

> \u26a0\ufe0f The memory role has the following hard constraints:
> - **Can only use local models**, does not support online or LAN.
> - **Tools are prohibited** (tool rounds fixed to 0).
> - **Streaming output is not supported**.
> - A specially fine-tuned memory model is needed for optimal performance.
> - Supports loading LoRA adapters.

### Invocation

The memory role is invoked on demand by the main role. No manual operation is required — once enabled, the agent automatically queries memory when needed.

---

## Security Role

**Disabled by default**, used for pre-screening high-risk content.

### Core Responsibilities

- Review user input for prompt injection
- Detect malicious code or sensitive information in tool output
- Assess risk of external content obtained from the web

### Security Review Mechanism

The system uses **two-layer security detection**:

1. **Logic-based detection**: Profanity filtering, structure corruption detection
2. **Security role model detection**: When the security role is enabled, a model performs in-depth review

### Configuration Notes

- When enabled, high-risk tools go through security review first.
- Execution pauses when risk is detected, pending user confirmation.
- Default temperature 0.2, max generation length 2048.
- Streaming output not supported.
- If no model is specified, automatically uses the main role's model.

---

## Custom Roles

Define custom roles through their system prompt to clarify role positioning. The main role will delegate subtasks to independently configured roles on demand, or can be explicitly invoked by **role name**.

### Use Cases

- Code Review: Delegate to an independent reviewer role
- Multi-language Translation: Delegate to a dedicated translation role
- Multi-perspective Analysis: Different roles use different models to analyze from different angles

### Configuration Notes

- Click **Add Role** to create a new delegate role.
- Each custom role can independently configure models, prompts, and tool sets.
- The main role can invoke custom roles by name to complete tasks.
- Default delegation mode is synchronous.

> Note: When the main role is using a local LLM, sub-roles can only reuse the same model or switch to HF / Online backends.

---

## Common Configuration Items

Each role supports the following configuration items:

### Model Selection

| Item | Description |
|------|-------------|
| Backend Type | Local model category (LLM / Multimodal / HF Standard) or online protocol |
| Model | Specific model selection |
| LoRA Mount Point | Attach a LoRA fine-tune adapter to the local model |
| Online Provider | Online model service provider |
| API Credentials | Bound API Key |
| LAN Address | Service URL for LAN mode |

### Prompt

| Item | Description |
|------|-------------|
| Role Prompt Template | Preset role behavior template |
| System Prompt | Custom role behavior instructions |

### Core Parameters

| Parameter | Description | Default |
|-----------|---------|-------------|
| Enable Tools | Whether to allow the role to call tools | Enabled |
| Temperature | Controls response randomness | 0.6 |
| Max Generation Length | Single task maximum token count (0=auto) | 0 |
| Max Context Length | Single task maximum context length (0=no limit) | Empty |

> Note: When the context length is empty, it means not to override the context length set in the model library. Recommended minimum 32K

### Advanced Inference Parameters

| Parameter | Description | Default |
|-----------|---------|-------------|
| Max Tool Rounds | Maximum tool call rounds per single task | 100 |
| Inference Timeout | Maximum wait time per inference | 600 s |
| Fast Mode | Skip feedback process before tool call | Disabled |
| Thinking Mode | Supports model-native thinking blocks | Enabled |

### Toolset Configuration

Each role can independently select allowed tools. Supports two modes:

- **Automatic Selection**: Dynamically select the most suitable tool within the selected tool range at runtime
- **Fixed Loading**: Always load all selected tools

> Note, the tools available during task execution strictly follow the selected range. Neither automatic selection nor tool discovery will call unselected tool groups.