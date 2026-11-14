# MCP External Tools

> v1.1.12
> 2026-11

MCP connects external tools to an agent. An agent can use them to read authorized project information or work with a local program you configure. After connecting a service, choose which tools each role may use.

Open **Settings → MCP**.

## Get started

1. Choose a service under **Quick Add**, or add a **Custom Service** if there is no preset.
2. Complete browser authorization or enter the connection details required by the service.
3. Wait for **Connected**, then open **Details** to check the tool catalog.
4. Return to the agent's **Runtime Settings**, select the MCP tools for the main or delegated role, and send a task.

**Connected does not mean the agent has permission to use the tools.** Connecting a service, selecting role tools, and approving task actions are separate steps.

## Choose a guide

| What you need | Guide |
| --- | --- |
| Connect a service shown on the page using your account | [Quick Add and Authorization](./connect.md) |
| Enter an MCP URL, token, or local program configuration | [Custom Services](./custom-service.md) |
| Let an agent use connected tools or help with setup | [Using MCP in Agents](./agent-tools.md) |
| Reconnect, edit, remove authorization, or troubleshoot | [Management and Troubleshooting](./manage.md) |

## Before you connect

- Remote services require a network connection. Accounts, permissions, plans, and charges are controlled by the provider. A Quick Add preset does not waive these requirements or grant access to your entire account.
- MCP calls may send task parameters to an external service. Connect only trusted services and select only the tools needed for the task.
- Local programs run on the **backend computer**, which may differ from the computer displaying the interface. Testing or enabling a service starts the program; this is not sandboxing or a security review.
- Browser authorization currently requires the desktop app connected to its local backend.
- Conversations using the same backend share service configurations. Check whether other conversations still use a service before editing or deleting it.

MCP lets agents call external tools. [External Access](../gateway/overview.md) lets external applications call ModxAI. They use different connections and credentials.
