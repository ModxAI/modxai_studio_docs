# MCP External Tools

> v1.1.12
> 2026-11

MCP connects external tools to agents, such as reading project information, looking up online documentation, or running a local program.

Open **Settings → MCP**.

## Get started

1. Search **Quick Add** and click a service card or its **＋** button. If there is no preset, click **Add** under **Custom Service**.
2. Complete browser authorization, or enter and save the connection settings.
3. Click the body of the added service card to open its details. Check that it is **Connected** and the tools you need are listed.
4. Return to the agent's **Runtime Settings**, select tools for the role, and send a task. See [Using MCP in Agents](./agent-tools.md).

**Connecting a service does not select tools for the agent.** Account authorization determines which external resources are accessible; role selections determine which tools the agent may use; task approval confirms a particular action.

## Choose a connection method

| Your situation | Guide |
| --- | --- |
| You found the service in Quick Add | [Quick Add and Authorization](./connect.md) |
| You have an MCP URL, token, or local program instructions | [Custom Services](./custom-service.md) |
| You want an agent to use tools or help configure a service | [Using MCP in Agents](./agent-tools.md) |
| You need to edit a connection, remove authorization, or resolve an error | [Management and Troubleshooting](./manage.md) |

## Before you connect

- Connect only trusted services. Remote tool calls may send task parameters to the provider. Account permissions, plans, and charges are controlled by that provider.
- Local programs run on the computer hosting the ModxAI service, referred to here as the **backend computer**. This is usually the computer running ModxAI; with a LAN backend, it may differ from the computer displaying the interface.
- Testing or starting a local service executes its program. It does not mean the program has been security-reviewed or runs in a sandbox.
- Browser authorization requires the desktop app connected to its local backend.
- Conversations connected to the same backend share service settings. Check whether other conversations depend on a service before editing, stopping, or deleting it.

MCP lets an agent call external tools. [External Access](../gateway/overview.md) lets external apps call ModxAI. Their connection methods and credentials are different.
