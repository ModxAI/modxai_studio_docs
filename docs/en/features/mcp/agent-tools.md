# Using MCP in Agents

> v1.1.12
> 2026-11

After connecting an MCP service, select tools for a role in the current conversation, then send a task. Connections can be shared, but check tool selections for each conversation and role.

## Select role tools

1. In **Settings → MCP**, click the added service card. Check that its details show **Connected** and the tools you need.
2. Return to the agent conversation and open **Runtime Settings** using the gear icon at the top right. Expand the main or delegated role under **Role Parameters**.
3. Under **Tool Set**, turn on **Enable Tools** and click **Configure Tools**. If tools are already selected, the button shows **N selected** instead.
4. Expand **MCP → service name**, select the tools you need, and click **Confirm**. If saving fails, the window keeps your selection; resolve the error and retry. Cancel does not apply the changes.
5. Send a small, clearly scoped task, such as reading one item from a specific project. Check the actual tool call and returned content before expanding the task.

Only the main and delegated roles can be configured with MCP tools. For delegated calls, both the main and delegated roles must have **Enable Tools** on and select the required MCP tools. Selecting them only for the delegated role is not enough.

The **Auto** checkbox selects tools as needed within your chosen scope. Uncheck it to use fixed selections. This is separate from auto approval and does not grant access to unselected tools.

## Check execution results

Tool calls show their status and results during a task. When approval is requested, review the action, target, and data to be sent before allowing it.

- A connected service does not mean the account can access every project or resource.
- The model's summary is not a substitute for the actual result. For changes, check the provider's own state when needed.
- A timeout, disconnection, or cancelled task may not undo an external action. Verify uncertain create, send, edit, or delete results before retrying.
- Check and select new tools when a service adds them. After deleting and re-adding a service, configure its tool selections again.

## Let an agent help with setup

You can ask the agent to check the environment and fill in settings when you have trusted connection instructions.

1. Turn off **Auto Approval** for the current conversation so you can review setup actions individually.
2. Open the main role's tool picker, select **MCP Management** under **Built-in → Execution**, and click **Confirm**. This group queries and registers services; it is not the external service's own tools.
3. Provide the official connection instructions, intended task, and allowed tool scope. Do not paste tokens, passwords, or client secrets into chat.
4. When the setup confirmation appears, check the service name, impact on shared settings, and tools allowed for this conversation. Expand details to inspect the command or URL.
5. Enter any required credentials in the private input fields. If browser authorization is needed, click **Allow** and follow the browser prompts, then wait for the registration result. Do not send credentials through ordinary chat.
6. After registration succeeds, check whether the connection and tool grant succeeded. If only the service was saved, select tools using the steps above, then send a simple task to verify the setup.

The confirmation may indicate:

| Scope | Meaning |
| --- | --- |
| Save only | Store the connection without adding tools to the current conversation |
| Named tools | Allow the current conversation's main role to use the listed tools |
| All tools discovered now | Allow the current conversation's main role to use this discovered set, excluding future additions |

Service settings are shared globally, but the setup grant applies only to the current conversation's main role, not other conversations or delegated roles. If tool selections changed during setup, the result may say activation was skipped. Check the current selections and add the required tools manually instead of registering again.

If setup requires downloads, dependency installation, or file creation, review the proposed source, location, and action first. Third-party programs should use a separate environment, not ModxAI's own runtime.

### Registration result is unclear

Ask the agent to check the result of that registration and inspect existing services in **Settings → MCP**. Do not immediately add it again: this can create duplicate connections or overwrite shared settings.

## Related guides

- [Role Configuration](../agent/roles.md)
- [Auto Approval and Safety](../agent/auto-approval.md)
- [Management and Troubleshooting](./manage.md)
