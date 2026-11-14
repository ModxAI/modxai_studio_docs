# Custom Services

> v1.1.12
> 2026-11

> This guide covers the upcoming v1.1.12 release.

Configure a connection manually when a provider supplies an MCP URL or local program instructions. Use the MCP endpoint, not the provider's homepage or a general API URL.

## Add a remote service

1. Open **Settings → MCP → Custom Service** and click **Add**.
2. Enter a **Server name** and set **Connection** to **Remote Service**. Use **Remote Service (Legacy SSE)** only when the provider requires it.
3. Enter the **Server URL** and choose **Authentication** according to the provider's instructions:

   | Authentication | What to enter |
   | --- | --- |
   | Automatic | The service response determines whether browser authorization is needed |
   | Browser Authorization | Authorize in your browser after saving |
   | Access Token | Enter the token in the **Access token** field |
   | Custom Headers | Enter the required header names and secret values |
   | None | Use only for a service that explicitly needs no authentication |

4. Open **Advanced Settings** if the service needs additional headers, a public OAuth client, or different timeouts.
5. Optionally click **Test Connection** to check the connection and tool discovery. A service that needs prior authorization may report an authentication error; save and authorize it first.
6. Click **Save**. A new service explicitly set to **Browser Authorization** continues to the browser flow. For other services needing authorization, click **Reconnect** on the card or **Authorize** in Details.
7. Confirm **Connected**, then open **Details** to check the tool catalog.

Copy the URL from the provider's official setup instructions. Put tokens and secrets in private fields, not in chat, the server name, or the URL.

## Local programs

A local MCP service runs on the **backend computer**. Prepare its executable and required environment using the provider's instructions before configuring the connection. Do not install third-party dependencies into ModxAI's own runtime environment.

1. Click **Add** under **Custom Service** and choose **Local Program**.
2. Enter the **Server name** and **Program**. Use an executable name or full path; prefer the full path if the program cannot be found.
3. Add each argument separately under **Arguments**, one entry per argument. Do not put a complete shell command in **Program**.
4. If needed, enter the **Working directory (optional, absolute path)** and **Environment variables** under **Advanced Settings**.
5. Click **Test Connection** and check the tool count. Click **Cancel Test** to stop the test.
6. Click **Save**, confirm **Connected**, and review the tool catalog.

For an existing trusted Python MCP service, for example, set **Program** to Python in that service's separate environment and put the script's full path in the first argument. Do not use the ModxAI application as a Python interpreter.

**Testing or enabling a service starts its program.** Launchers such as `npx` and `uvx` may also download and execute code. Check the source, version, and installation requirements first. Registering a service does not install missing dependencies.

## Testing versus saving

- **Test Connection** checks the connection and discovers tools. It does not call business tools or save your changes. A local test may start a temporary process.
- **Save** stores the configuration and attempts to connect. It does not prove that every tool can run successfully.
- After **Connected** appears, you still need to [select tools for a role](./agent-tools.md#select-role-tools).

## Advanced settings and saved credentials

| Setting | Use |
| --- | --- |
| Connection Timeout (Seconds) | Time allowed to connect: 5–120 seconds; custom services default to 20 |
| Call Timeout (Seconds) | Time allowed for one tool call: 5–1800 seconds; default 120 |
| Authorization Server, Public Client ID | Enter only when required by the provider; do not enter a client secret |
| Request headers, Environment variables | Use the required names and enter values in the private fields |

When editing a saved service:

- Leave the token blank to keep its saved value. Select **Clear Saved Token** to remove it.
- Existing header and environment-variable values are kept unless you select **Replace saved value** or remove the entry. Selecting replacement and leaving the value blank saves an empty string; it does not keep the old value.
- If another operation changed the configuration, click **Load latest configuration (replaces unsaved changes)**, review it, and edit again.
- Editing a shared service affects other conversations. After changing its URL or authentication, recheck the connection and permissions instead of assuming the old authorization still applies.

## Related guides

- [Quick Add and Authorization](./connect.md)
- [Using MCP in Agents](./agent-tools.md)
- [Management and Troubleshooting](./manage.md)
