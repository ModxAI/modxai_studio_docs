# Quick Add and Authorization

> v1.1.12
> 2026-11

Quick Add supplies the service's connection settings. If account authorization is needed, ModxAI opens your browser so you can choose an account and access scope.

## Before you start

- Browser authorization requires the desktop app connected to its local backend and network access to the provider's website.
- Have the required account ready. Workspace, project, or organization access may require administrator approval.
- Local program presets require their runtime environment. For example, Playwright uses Node.js/npm and may download a package on first launch. Check its requirements first.

## Add a service

1. Open **Settings → MCP**. Search by name or filter by category in **Quick Add**; use the page controls to see more services.
2. Click the service card or its **＋** button. Added services show reconnect, edit, and on/off controls, so you do not need to add them again.
3. If authorization is required, wait for the browser to open and check the account, workspace, and permissions on the provider's page. **Authorization Details** in the ModxAI dialog shows the authorization origin and requested scope.
4. Return to ModxAI after authorization and wait for saving to finish. You do not need to enter client parameters or click Save again.
5. Click the body of the added service card to open details. Confirm **Connected** and inspect **Tools**. Scrolling within a long tool list loads more entries.
6. Follow [Select role tools](./agent-tools.md#select-role-tools) before asking the agent to perform a task.

A new browser-authorized preset is saved only after authorization succeeds. Clicking **Cancel** or closing the ModxAI authorization dialog during authorization does not complete the addition. Closing only the browser tab does not cancel the app's waiting flow.

Presets that do not require browser authorization are saved directly and then try to connect, such as Context7 and local Playwright. Saving successfully does not mean the connection succeeded; check the status and tool list in details.

## Check access scope

- The GitHub preset uses the read-only service. Select repositories on the browser page; organization repositories may require administrator approval. This entry is not for creating commits or modifying repositories.
- Other services use the permissions of the account and authorization page. Quick Add does not grant access to every resource or guarantee a free allowance.
- A service missing from the list can only be tried as a [custom service](./custom-service.md) if it supplies a compatible MCP endpoint or local launch method. An ordinary webpage is not an MCP endpoint.

## Authorization did not finish

| Symptom | What to do |
| --- | --- |
| The browser did not open, or the authorization tab was closed | Click **Retry** in the ModxAI authorization dialog to reopen it. If the flow has failed, Retry starts a new authorization attempt |
| Authorization was denied or expired | Start authorization again and check the account and requested scope |
| A local-backend requirement is shown | Use the desktop app connected to its local backend; do not bypass browser security controls |
| Authorization succeeded but saving failed | Read the dialog error and check whether the service already exists before retrying, to avoid duplicates |
| The saved service failed to connect | Click its card for the specific error and follow [Management and Troubleshooting](./manage.md) |

If a device code is displayed, use it only on the provider's page for this authorization. Do not send it in chat.

## Remove authorization

Click the added service card, then **Revoke Authorization** at the bottom of details and confirm. This removes the locally stored authorization and disconnects the service while retaining its configuration and tool selections.

**It does not revoke access at the provider.** To remove that access, also use the provider's account settings. To connect again, leave the service switch on, click **Authorize** in details, and wait for authorization, saving, and connection to finish.
