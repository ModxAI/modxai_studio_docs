# Using MCP in Agents

> v1.1.12
> 2026-11

After connecting an MCP service, select which tools a role may call. You can also let an agent help configure a service, but saving a connection and granting tool access are separate actions.

## Select role tools

1. In **Settings → MCP**, confirm that the service is enabled, shows **Connected**, and has a tool catalog.
2. Return to the agent page and open **Runtime Settings** using the gear icon at the upper right.
3. Select the main or delegated role, turn on **Enable Tools**, and click **Configure Tools** under **Tool Set**.
4. Find **MCP Tools**, select the tools needed for the task by service, and confirm the selection.
5. Send a narrowly scoped task, such as reading information from a specific project. Check the tool result before expanding the task.

MCP tools can currently be configured for the main and delegated roles. Do not assume the evaluate, summary, memory, or security roles can use them.

With **Auto** enabled, the agent chooses tools within the selected scope. Fixed mode loads the selected tools. Neither mode grants access to every tool just because a service is connected.

## Check task results

Tool calls in the task show their status and results. When approval is requested, review the target, action, and data being sent before approving or rejecting it.

- A tool appearing in the catalog does not mean your account has access to every target.
- Check the actual result even after a successful tool call. The model's summary does not replace the provider's state.
- After a timeout, disconnection, or cancellation, an external write may already have occurred. Check the target before retrying instead of submitting the same action repeatedly.
- When a service adds tools, review the role's tool selection again. Do not assume the previous selection includes new tools.

## Let an agent help with setup

Use this when you have trusted setup instructions and want help preparing the connection.

1. Turn off **Auto Approval**, then enable **List MCP Servers** and **Register MCP Server** under **MCP Management** for the main role.
2. Give the agent the service's purpose, official setup instructions, and allowed tool scope. Share only non-sensitive configuration; do not paste tokens, passwords, client secrets, or browser codes into chat.
3. Review the service name, connection configuration, changes, and tool-access scope in the approval card. Updating an existing service affects other conversations using it.
4. Enter required credentials in the card's private input fields. If account authorization is needed, approve the request and complete the browser flow. Do not supply credentials through ordinary chat.
5. Wait for the registration result, then check the service in **Settings → MCP**. Newly authorized tools become available on a subsequent model request, not immediately within the same batch.

The approval may show one of three scopes:

| Scope | Meaning |
| --- | --- |
| Save only | Save the connection without granting tools to this conversation |
| Named tools | Grant only the listed tools to this conversation's main role |
| All tools discovered now | Grant the current discovery snapshot, excluding future additions |

The service configuration is shared, but this grant applies only to the current conversation's main role. It does not also grant access to other conversations or delegated roles.

A local service's program and dependencies must be prepared first. If the agent needs to download software, install dependencies, or create files, it should explain those actions and obtain your permission separately. Registration itself does not install dependencies. Keep the third-party environment separate from ModxAI's own runtime.

### If registration status is unclear

If registration times out or its result is unclear, ask the agent to query the **original operation's status** and check existing services in Settings. Do not immediately register again: this can create duplicate connections or overwrite shared configuration.

## Related guides

- [Role Configuration](../agent/roles.md)
- [Auto-Approval and Safety](../agent/auto-approval.md)
- [Management and Troubleshooting](./manage.md)
