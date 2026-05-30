# Channel Connection

> v1.1.0
> 2026-05

Channel connections allow you to interact with the local agent service through external instant messaging platform bots (Telegram, Discord, DingTalk, Feishu). You can send tasks, switch sessions, and adjust configuration from anywhere via chat messages — just like in ModxAI Studio.

## Table of Contents

- [Supported Platforms](#supported-platforms)
- [Quick Start](#quick-start)
- [Platform Configuration](#platform-configuration)
- [Pairing & Security](#pairing--security)
- [Control Commands](#control-commands)
- [Session Management (Admin)](#session-management-admin)
- [Advanced Settings](#advanced-settings)
- [Notes](#notes)

---

## Supported Platforms

| Platform | Credentials Required | Pairing Method |
|----------|---------------------|----------------|
| **Telegram** | Bot Token | `/start <pairing_code>` |
| **Discord** | Bot Token | `/start <pairing_code>` |
| **DingTalk** | Client ID + Client Secret | `/start <pairing_code>` |
| **Feishu** | App ID + App Secret | `/start <pairing_code>` |

---

## Quick Start

### Step 1: Configure Platform Credentials

1. Create a bot on the target platform (e.g., Telegram) and obtain the Bot Token.
2. Open the Channel Connection page and select the corresponding platform in "Chat Bot."
3. Enter the credentials (Bot Token or App ID + App Secret).
4. Click **Save Settings**.

### Step 2: Start the Service

Click **Start Connection**. The system will:

1. Verify the credentials are valid.
2. Launch the gateway subprocess.
3. Register the bot adapter.
4. Display the service status as "Connected."

### Step 3: Pair a Device

After starting, the page displays a pairing command in the format:

```
/start <pairing_code>
```

Send the above command to the bot on the target platform to complete pairing. After pairing, that chat/channel/session can interact with the agent.

### Step 4: Start Chatting

After successful pairing, send messages directly on the platform to trigger agent responses. Responses follow the roles, models, and tool strategies configured in your Agent Workbench.

---

## Platform Configuration

### Telegram

| Item | Description |
|------|-------------|
| Bot Token | Token obtained from @BotFather |

### Discord

| Item | Description |
|------|-------------|
| Bot Token | Token obtained from the Discord Developer Portal |

### DingTalk

| Item | Description |
|------|-------------|
| Client ID / RobotCode | DingTalk enterprise internal robot Client ID |
| Client Secret | DingTalk enterprise internal robot Client Secret |

> **Note**: DingTalk credentials (Client ID and Client Secret) must be **filled in as a pair**. An error is shown if only one is provided.

### Feishu

| Item | Description |
|------|-------------|
| App ID | Feishu enterprise self-built robot App ID |
| App Secret | Feishu enterprise self-built robot App Secret |

> **Note**: Feishu credentials (App ID and App Secret) must be **filled in as a pair**. An error is shown if only one is provided.

---

## Pairing & Security

### Pairing Process

1. After starting the service on the Channel Connection page, the system automatically generates a **pairing code**.
2. The page displays the pairing command `/start <pairing_code>`. Click the **Copy** button to copy it.
3. Send `/start <pairing_code>` to the bot on the target platform.
4. On successful pairing, the bot replies with a confirmation message.
5. That chat/channel/session is added to the allowlist, and messages can be sent directly afterward.

> **Security Mechanism**: Only successfully paired sources can send messages to the agent. Messages from unpaired sources are ignored.

---

## Control Commands

After pairing, you can send commands starting with `/` to the bot to control behavior. Commands are divided into **Public Commands** and **Admin Commands**.

Parameter format: `<required>` `[optional]`

### Public Commands

The following commands require no admin privileges:

| Command | Description |
|---------|-------------|
| `/help` | Show help for all available commands |
| `/status` | Display current session status (role, model, tool configuration, etc.) |
| `/stop` | Stop the currently generating response |
| `/list` | List all sub-sessions under the current control source |
| `/list_roles` | List all roles and their model configurations |
| `/list_models` | List available local and online models |
| `/list_tools [role]` | List tool groups and their enabled status, optionally for a specific role |

### Admin Commands

Enter admin mode with `/admin <password>` to execute:

**Session Management:**

| Command | Description |
|---------|-------------|
| `/new [name]` | Create a new sub-session and switch to it |
| `/change <id\|name>` | Switch to the specified sub-session |
| `/rename <name>` | Rename the current session |
| `/delete <name>` | Delete the specified sub-session |
| `/clear` | Clear the current session context |

**Parameter Configuration:**

| Command | Description |
|---------|-------------|
| `/model <id\|Provider/Model> [role]` | Set a model for a role |
| `/system [role] <text>` | Set the system prompt for a role |
| `/temperature <value> [role]` | Set the temperature parameter |
| `/max_tokens <value> [role]` | Set the maximum output token count |
| `/thinking <on\|off> [role]` | Enable/disable thinking mode |
| `/role_timeout <seconds> [role]` | Set inference timeout |
| `/tools_rounds <value> [role]` | Set the maximum number of tool call rounds |

**Tool Control:**

| Command | Description |
|---------|-------------|
| `/switch_tools <on\|off> [role]` | Enable/disable tool calling |
| `/auto_tools <on\|off> [role]` | Enable/disable automatic tool selection |
| `/tools <id\|all> <on\|off> [role]` | Enable/disable tools by group |
| `/allow_risky_tools <on\|off>` | Toggle high-risk tool policy |

**Workspace & Execution:**

| Command | Description |
|---------|-------------|
| `/dir <relative_path>` | Set the workspace subdirectory |
| `/yolo <on\|off>` | Enable/disable auto approval |
| `/switch_role <summary\|memory\|security\|evaluate> <on\|off>` | Toggle built-in roles |
| `/approve` | Approve the currently pending operation |
| `/reject` | Reject the currently pending operation |

### Admin Authentication

Admin authentication is **in-process cached**; re-authentication is required after a gateway service restart.

| Command | Description |
|---------|-------------|
| `/admin <password>` | Enter admin password to enter admin mode |
| `/admin exit` | Exit admin mode |

> **Initial Setup**: If no admin password has been configured, the system automatically generates an invisible password on first startup. You can modify it in "More Settings."

---

## Session Management (Admin)

### Sub-session Mechanism

Each external platform source (chat/channel/session) can manage **multiple sub-sessions**:

- Default session: Automatically created after pairing; send and receive messages directly.
- Sub-sessions: Created via `/new`; can be independently configured and switched.
- `/list` lists all sub-sessions; switch by ID.

### Session Isolation

Each sub-session has independent context, allowing different tasks to run simultaneously without interference.

---

## Advanced Settings

In the Channel Connection page, you can configure the following advanced options:

- **Auto-reconnect**: Automatically attempt to reconnect if the connection drops.
- **Response Language**: Set the language for bot responses.
- **Max Message Length**: Limit on the length of messages the bot can receive.
- **Admin Password**: Set or modify the admin password.

---

## Notes

- Ensure the gateway service is running properly; check the connection status on the page.
- After a credential change, the service needs to be restarted.
- Different platforms may have different message format limitations.
- Paired device list can be managed on the Channel Connection page.
