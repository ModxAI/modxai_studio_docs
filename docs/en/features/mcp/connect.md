# Quick Add and Authorization

> v1.1.12
> 2026-11

> This guide covers the upcoming v1.1.12 release.

Quick Add provides connection defaults for a service. Services that need account authorization open your browser so you can choose the account and permissions.

## Prerequisites

- For browser authorization, use the desktop app connected to its local backend, with a browser that can reach the provider's website.
- Have the required account and permissions. Organization resources may require administrator approval.
- Local-program presets also require their program and dependencies on the backend computer. See [Local programs](./custom-service.md#local-programs).

## Add a service

1. Open **Settings → MCP**.
2. Search for the service under **Quick Add**, or filter by category.
3. Click the add button on its card.
4. If authorization is required, review the origin and requested permissions under **Authorization Details**, then complete the browser flow. If a device code appears, enter it only where the provider requests it, not in chat.
5. Return to ModxAI and wait for authorization and saving to finish. Confirm that the card shows **Connected**.
6. Open **Details** to view the tools, then [select tools for a role](./agent-tools.md#select-role-tools).

A new browser-authorized preset is saved only after authorization succeeds, then attempts to connect. Closing the authorization dialog or clicking **Cancel** does not complete the addition. When reauthorizing an existing service, wait for the save result as well.

Presets that do not require browser authorization save and attempt to connect directly. A local-program preset still depends on the environment on the backend computer.

## Check the access scope

- A provider may limit access to selected workspaces, projects, or repositories. Check the scope on its authorization page.
- The GitHub Quick Add preset uses a read-only service. Select the allowed repositories and complete installation as prompted; organization repositories may require administrator approval. Do not expect it to create commits or modify repositories.
- Available presets and tools depend on the app version and the provider's response. For a service not shown, follow its setup instructions to add a [Custom Service](./custom-service.md). Its name alone does not establish compatibility.

## If authorization does not finish

| What you see | What to do |
| --- | --- |
| The browser did not open or was closed | Click **Retry** in the authorization dialog to continue |
| Authorization was denied or expired | Start again and check the account and permissions you allow |
| A local-backend requirement is shown | Use the desktop app connected to its local backend; do not disable browser security to bypass this restriction |
| A registered client is required | Check the provider's instructions. If a public client is supplied, enter it in Advanced Settings; a client secret is not a Public Client ID |
| Authorization succeeded but connection failed | Read the error on the card or in Details, then follow [Management and Troubleshooting](./manage.md) |

## Remove authorization

Open the service's **Details**, click **Revoke Authorization**, and confirm. This removes locally stored authorization and disconnects the service while keeping its configuration and tool selections.

**This does not revoke access at the provider.** To revoke that access too, remove the authorization in the provider's account settings. Authorize again before using the service later.
