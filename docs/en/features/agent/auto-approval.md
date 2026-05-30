# Auto Approval & Security

> v1.1.0
> 2026-05

During execution, the agent may need to perform high-risk operations such as executing terminal commands or modifying files. The auto-approval and security mechanism helps balance efficiency and safety.

## Table of Contents

- [Auto Approval](#auto-approval)
- [Security Review](#security-review)
- [Pause Confirmation Scenarios](#pause-confirmation-scenarios)
- [Command Blacklist](#command-blacklist)
- [Security Role](#security-role)

---

## Auto Approval

When auto approval is enabled, operations requiring confirmation are automatically approved, eliminating the need for manual intervention.

### Toggle Control

- An **Auto Approval toggle** is located above the input box.
- Disabled by default; session-independent (toggling one session does not affect others).
- A confirmation message is shown when enabling or disabling.

### Use Cases

| Scenario | Recommendation |
|----------|----------------|
| Trusted workspace + clear task | Enable auto approval for efficiency |
| Unfamiliar codebase | Disable and review each step |
| System configuration tasks | Disable to avoid accidents |
| Batch file operations | Can be enabled, but test in a sandbox workspace first |

### Auto Approval Behavior

When enabled, the system will:
- Automatically allow command execution (those compliant with security policies)
- Automatically allow file write operations
- Automatically resume paused tasks

**Commands on the blacklist are always skipped**, even in auto-approval mode.

---

## Security Review

The system performs **two-layer security detection** on user input and tool output.

### Layer 1: Input Check

Before user messages enter the agent, the system checks:

| Check Item | Description |
|------------|-------------|
| Sensitive Word Filtering | Filters inappropriate content |
| Prompt Injection Detection | Prevents jailbreaking or behavior modification through user input |
| Structure Corruption Detection | Detects JSON/XML/code injection and special token abuse |
| Tool Misuse Detection | Prevents calling illegal tools or out-of-bounds access |

### Layer 2: Output Check

Before tool execution results are returned, the system checks:

| Check Item | Description |
|------------|-------------|
| Malicious Code Detection | Scans for dangerous code in execution results |
| Information Leakage Detection | Detects API keys and private information |
| External Content Risk | Security assessment of web-crawled, document-read, and other external content |

### Security Pause

When a risk is detected, the agent enters a **Security Pause** state:

- The interface displays the specific risk reason.
- Two options are available:
  - **Continue**: Confirm and allow the operation to proceed.
  - **Stop**: Immediately halt the current task.

---

## Pause Confirmation Scenarios

The agent pauses for confirmation in the following scenarios:

### Command Execution Confirmation

The agent pauses before executing a terminal command, displaying:

- Command source (terminal command)
- Working directory
- Command preview
- Risk indication (e.g., installing dependencies, modifying files, accessing the network)

Options: **Allow** / **Skip**

### Tool Round Limit

Pauses when the current tool call round limit is reached. You may continue or stop.

### Repeated Execution Warning

Pauses when the system detects that the current path may be stuck in a loop, asking for confirmation to continue.

### Model Stalled

Pauses when the agent has not made meaningful progress; you can confirm to let it try again.

### Human Decision

When the agent uses the `ask_human` tool to request a user decision, a decision dialog appears:

- Displays the question title and options.
- Supports multi-question pagination.
- Supports **recommended option** markers.
- Supports **custom input** as an alternative to preset options.
- After submission, the agent continues based on your choice.

---

## Command Blacklist

The command blacklist is the last line of defense against dangerous commands.

### Blacklist Features

- Supports **exact match**: e.g., `rm` matches `rm`
- Supports **prefix match**: e.g., `rm` matches `rm file.txt`
- Supports **wildcards**: `*` and `?`
- Automatically covers argument variants: a single `rm` rule can block `rm -rf /` and its variants
- Recognizes real subcommands in chained commands, pipes, and wrapper commands

### Default Blacklist

The system presets common dangerous commands: `rm`, `del`, `format`, `shutdown`, `taskkill`, `dd`, `mkfs`, `git clean`, `git reset --hard`, etc.

### Custom Blacklist

Edit the command blacklist in Advanced Settings \u2192 Security group. Separate multiple commands with commas or newlines.

> ⚠️ Commands on the blacklist are **always rejected**, regardless of auto-approval or manual confirmation mode.

---

## Security Role

The security role is an optional role component. The security review described above is only performed when it is enabled.

### Enabling

In the right configuration panel, expand "Role Parameters" \u2192 "Security Role" \u2192 enable and configure the model.

### Relationship with Basic Security

- Basic security checks (sensitive word filtering, etc.) are always active and do not require a security role.
- The security role provides additional **semantic-level** security analysis.
- It is recommended to enable the security role when processing high-risk content.
- If no model is specified for the security role, it automatically uses the main role's model.
