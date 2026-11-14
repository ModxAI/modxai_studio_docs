# Management and Troubleshooting

> v1.1.12
> 2026-11

Open **Settings → MCP** and click the body of an added service card to view its status, errors, and tools. The card's colored edge indicates status; hover over the card for the status text.

## Everyday management

| Where | Action and result |
| --- | --- |
| Refresh icon at the top right of Quick Add | Refresh connections and tool catalogs for all enabled services; disabled services do not start |
| Circular arrow on an added card (**Reconnect**) | Reconnect that service. If browser authorization is missing, open the authorization flow. Turn on a disabled service first |
| Pencil icon on the card (**Edit**) | Edit connection settings and save to apply |
| Card switch (**Stop / Start**) | Disable or enable the service without deleting its configuration or existing tool selections |
| Body of the card | Open details for status, errors, and tools. Scroll the tool list to load more; expand **Connection Info** for connection information |
| **Authorize / Revoke Authorization** at the bottom of details | Complete browser authorization, or confirm removal of local authorization. This does not revoke access at the provider |
| **Delete** at the bottom of details | Confirm deletion of the connection and catalog. All conversations lose access to this connection; existing messages and files remain |

Deleting a saved preset connection makes its Quick Add card available again; it does not remove the preset entry. After adding it again, select tools for the roles again.

ModxAI refreshes enabled services once at startup. It does not then refresh catalogs on a fixed interval or continuously retry failed connections. Reconnect manually after network recovery; merely switching pages does not reconnect all services.

For a failed older local preset, details may offer **Update and Connect**. This updates launch arguments from the preset, adds missing default environment variables, increases the connection timeout if needed, and retries. The card's ordinary **Reconnect** action does not replace those settings.

## Understand status

| Status | What to do |
| --- | --- |
| Disabled | Turn on the card's switch when you need the service |
| Not connected / Connecting | Wait for the result; if it does not finish, inspect settings and errors |
| Connected | Inspect tools and configure the role; this does not mean the business task is complete |
| Authentication required | Authorize as prompted, or edit the service to supply credentials |
| Connection failed | Read the specific error in details, correct it, and reconnect |
| Stopping | Wait for stopping to finish |

A failed catalog refresh preserves the last complete catalog, so visible tools do not prove that the connection is working. A tool marked with an unsupported schema or external reference cannot be used directly for a task.

## Common problems

### Program or working directory not found

Check that the computer hosting ModxAI has the required program and directory. Try a full path in **Program**. With a LAN backend, a path on the computer displaying the interface is not automatically a valid backend path.

### Connection timeout or failure

For local services, check the program, arguments, and dependencies. For remote services, check the MCP URL, network, and authentication. If the service genuinely needs more time, adjust **Connection Timeout** in the editor's **Advanced Settings**.

### Authentication failure, missing credentials, or access denied

Use the card's **Edit** action to supply tokens or headers. For browser-authorized services, complete authorization in details. Also check that the account can access the intended workspace, project, or repository. Organization resources may require administrator approval.

### Connected, but the agent cannot find a tool

1. Click the service card and check that the target tool is listed and not marked unsupported.
2. In the current conversation's role settings, check **Enable Tools** and the tool selections. Click **Confirm** after changing them.
3. For a delegated role, also check that the main role allows those tools.
4. After agent-assisted setup, check whether the result only saved the service or skipped activation. Select the required tools and send a simple task.

Do not keep adding the same service to solve a missing tool selection.

When using an external app or bot, also check the high-risk tool restrictions in [External Access](../gateway/overview.md). Selecting a tool for a role does not bypass those restrictions.

### Tool list failed to load or the catalog is invalid

For a loading error inside the tool list, click **Retry** there. If details report a catalog refresh failure, try reconnecting. An oversized catalog or unsupported tool definition requires a compatible catalog from the provider; repeatedly selecting tools will not resolve it.

### Backend dependencies are incomplete

This differs from a missing external program: the former concerns ModxAI's runtime, while the latter concerns the third-party service's environment. Keep the error message and check the app's installation instructions. Do not mix third-party packages into the ModxAI environment to resolve it. If the error persists after a restart, report it with a redacted message.

### A timed-out action may have succeeded

Inspect the original call result and verify the provider's actual state. Do not repeat create, send, edit, or delete actions before checking. Cancelling a task, stopping a connection, or removing local authorization does not undo external actions that already happened.

## Related guides

- [Custom Services](./custom-service.md)
- [Using MCP in Agents](./agent-tools.md)
- [Auto Approval and Safety](../agent/auto-approval.md)
