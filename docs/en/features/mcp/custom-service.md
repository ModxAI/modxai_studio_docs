# Custom Services

> v1.1.12
> 2026-11

Configure a service manually when its provider supplies an MCP URL or local launch instructions. Use the official instructions, not the provider's homepage or a general API URL.

## Add a remote service

1. Open **Settings → MCP** and click **Add** beside **Custom Service**.
2. Enter a **Server name** and change **Connection** to **Remote Service**. Choose **Remote Service (Legacy SSE)** only when the provider requires that transport.
3. Enter the **Server URL** and choose **Authentication** according to the provider's instructions:

   | Method | What to do |
   | --- | --- |
   | Automatic | Save first; if the connection requires authentication, complete authorization afterward |
   | Browser Authorization | A new service opens the authorization flow after saving |
   | Access Token | Enter the token in the **Access token** field that appears |
   | Custom Headers | Click **Add Entry** and enter each header's **Name** and **Secret value** |
   | None | Use only when the provider explicitly requires no authentication |

4. Expand **Advanced Settings** if you need extra headers or different timeouts. Enter an Authorization Server and Public Client ID only when the provider requires them; do not enter a client secret.
5. Optionally click **Test Connection** at the bottom left of the drawer. Its result appears at the bottom. Use **Cancel Test** to stop waiting.
6. Click **Save** at the bottom right. A new service using **Browser Authorization** then opens authorization. For **Automatic**, or after editing an existing service, click its card and **Authorize** in details if authorization is needed.
7. In details, confirm **Connected** and inspect the **Tools** list.

Testing does not replace account authorization. If the test requires authentication, save the service and complete authorization first. Enter tokens and secrets only in private fields, not in chat, the server name, or the URL.

## Local programs

A local program runs on the computer hosting the ModxAI service, not necessarily the computer displaying the interface. Prepare the program and its environment according to its instructions. Do not install third-party dependencies into ModxAI's own runtime environment.

1. Click **Add** under **Custom Service**, leaving **Connection** set to **Local Program**.
2. Enter a **Server name** and **Program**. Use an executable name or full path, or select a file with the folder button beside the field.
3. Click **Add argument** under **Arguments** and enter one argument per input. Do not put an entire terminal command in **Program**.
4. Expand **Advanced Settings** as needed to set the **Working directory**, or click **Add Entry** under **Environment variables** to enter names and secret values.
5. Click **Test Connection** and check that it connects and discovers tools, then click **Save**.
6. Click the saved service card and check its connection status and tool list in details.

For a trusted Python MCP script, for example, set **Program** to the Python executable in that script's separate environment. Click **Add argument** and enter the script's full path. With a LAN backend, these paths must be valid on the backend computer.

**Testing or starting a service executes its program.** Launchers such as `npx` and `uvx` may download and run code. Check the source, version, and installation requirements. If Node.js, Python, or another runtime is missing, prepare it according to the service's instructions; ModxAI has no dedicated MCP installation wizard.

## Testing versus saving

- **Test Connection** checks connectivity and tool discovery without calling business tools or saving changes. A local test may start a temporary process.
- **Save** stores the configuration. Enabled services try to connect; disabled services remain disabled.
- **Connected** does not guarantee permission to use every tool on every resource. You must also [select role tools](./agent-tools.md#select-role-tools).

## Advanced settings and credentials

The default connection timeout is **20 seconds**, adjustable from 5 to 120 seconds. The default call timeout is **120 seconds**, adjustable from 5 to 1800 seconds. Presets may use a different connection timeout.

To edit an existing service, click its pencil icon (**Edit**):

- Leave **Access token** blank to keep the saved token, or enter a new token and save to replace it. Select **Clear Saved Token** to remove it.
- Saved header and environment-variable secret values are not displayed. Leave their inputs untouched to keep them. Entering a new value replaces the saved value when you save; typing and then clearing the input saves an empty string. To delete an entry, use its remove icon and save.
- To discard a value you just typed, click **Cancel** and reopen the editor. Clearing the input does not restore the previous value.
- URL query values may be hidden. Leaving the address unchanged preserves them; when changing it, enter the complete URL with all query values again.
- If a configuration conflict appears, click **Load latest configuration (replaces unsaved changes)**, then review and edit again. This replaces your current unsaved changes.

Editing a shared service affects other conversations. Check the connection and authorization again after changing its URL or authentication method.

## Related guides

- [Quick Add and Authorization](./connect.md)
- [Using MCP in Agents](./agent-tools.md)
- [Management and Troubleshooting](./manage.md)
