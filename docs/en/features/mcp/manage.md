# Management and Troubleshooting

> v1.1.12
> 2026-11

> This guide covers the upcoming v1.1.12 release.

Open **Settings → MCP** to check status, reconnect, or edit a service. Read the error on its card or in **Details** before choosing an action.

## Manage connections

| Action | Effect |
| --- | --- |
| **Refresh** at the upper right of Quick Add | Refresh connections and tool catalogs for enabled services; disabled services are not started |
| **Reconnect** on a service card | Reconnect that service, continuing to browser authorization if needed |
| **Edit** | Change connection details; saved changes affect other conversations using the service |
| **Stop / Start** switch | Disable or enable the service without deleting its configuration |
| **Details** | View connection status, tool descriptions, and connection information |
| **Revoke Authorization** | Remove local authorization and disconnect, keeping configuration and tool selections; does not revoke provider-side access |
| **Delete** | Remove the configuration and tool catalog; all conversations lose access to this service, while existing messages and files remain |

The app refreshes enabled services once at startup. It does not refresh on a fixed interval or keep retrying failed connections. Use **Refresh** or **Reconnect** after network recovery or a service change.

If an older preset fails and Details shows **Update and Connect**, that action updates its connection defaults and retries. Check the resulting connection status.

## Read the status

| Status | Meaning and next step |
| --- | --- |
| Disabled | The service is off; use the switch to start it when needed |
| Not connected / Connecting | The connection is not complete; wait for the result or check configuration |
| Connected | A connection is established; check the catalog and role selection next. A business task is not necessarily complete |
| Authentication required | Supply credentials or authorize again |
| Connection failed | Read the specific error, correct it, and reconnect |
| Stopping | Wait until stopping finishes before acting again |

If catalog refresh fails, the last complete catalog is kept. **Visible tools do not prove the current connection works.** Resolve connection or catalog errors first. A tool marked as having an unsupported schema or external reference cannot be treated as usable.

## Troubleshooting

### Program or working directory not found

On the backend computer, check that the program is installed, its path is correct, and the specified directory exists. Use the program's full path if needed. A path on the current computer may not exist on the backend computer.

### Connection timed out or failed

Check the local program and arguments, or the remote MCP URL, network, and authentication method. Increase **Connection Timeout** only if the service genuinely needs more time; it will not fix a wrong URL or missing permission.

### Authentication failed, credentials missing, or access denied

Use **Edit** to supply the token or headers, or authorize again in the browser. Confirm that the account can access the target workspace, project, or repository. Organization resources may need administrator approval. See [Quick Add and Authorization](./connect.md).

### Connected, but the agent cannot find a tool

Check these in order:

1. **Details** has a tool catalog and the tool is supported.
2. **Enable Tools** is on for the main or delegated role, and the required tool is selected.
3. You edited the role and configuration used by the current task.
4. If the agent just registered and activated tools, a subsequent model request has started.

Do not repeatedly add the same service to fix a missing role selection.

### Invalid catalog, catalog limit, or unsupported tool

Confirm that the URL points to an MCP service and check its version and transport. Selecting role tools cannot fix an oversized catalog or unsupported tool definition. The provider must supply a supported catalog; refresh after it changes.

### Backend dependencies are incomplete

Repair the ModxAI runtime as prompted. This differs from an external program being missing: the former concerns the app's runtime, while the latter concerns the service's own environment. If process supervision is not ready, try restarting the backend.

### An operation timed out and the result is unclear

Read the original tool-call or registration result and check the provider's actual state. Do not repeat creates, sends, updates, or deletions until the outcome is clear. Cancelling a task or stopping a connection does not undo an external action that already occurred.

## Related guides

- [Custom Services](./custom-service.md)
- [Using MCP in Agents](./agent-tools.md)
- [Auto-Approval and Safety](../agent/auto-approval.md)
