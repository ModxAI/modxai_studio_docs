# Workspace Browser

> v1.1.11
> 2026-11

The workspace browser lists directories and files in the current workspace, including files the agent has not read or changed. Preview results in the app, open files, or save a copy locally.

## Open Workspace Resources

1. Open **Runtime Settings** on the agent page and check **Workspace Root**. Select your project directory if needed.
2. Click **Workspace Resources** at the top of the page.
3. Expand directories on the left and select a file. You can search by filename, filter by type, or refresh the current directory.

Drag the drawer edge to adjust its width. If a newly generated file is missing, refresh and check the workspace path used by the current conversation.

## Resource Preview

- Code, plain text, Markdown, and HTML support built-in preview, with a **10 MB** text preview limit.
- Images, audio, and video can be previewed depending on format, with a **32 MB** media preview limit.
- For unsupported or oversized files, use **Open**, or **Download locally** and view them in a suitable application.

Previewing does not require a loaded model and does not replace a file editor.

### HTML Interaction

HTML opens in static isolated preview by default. Scripts and external resources are disabled.

1. Check that you trust the file's source and code before clicking **Trust and Enable Interaction**.
2. Check the file path in the confirmation dialog. Confirming enables scripts, downloads, and page dialogs.
3. Click **Return to Static Preview** when interaction is no longer needed.

Interactive preview remains isolated, with network and main-app access blocked. Changing files or modifying the file requires confirmation again. Do not enable interaction merely to inspect an unfamiliar file.

## Tabs

Selecting a file opens a preview tab. Close individual tabs, or use the tab menu to close other tabs, tabs to the right, or all tabs. Back and forward buttons navigate previously viewed tabs.

Changing the main workspace clears the previous workspace's tabs and previews.

## Work Notes and Task Association

The agent can save handoff notes containing results, paths, and unresolved items. Later tasks can refer to these notes when reading relevant history. This does not put every historical conversation into the current context.

Choose a read mode under **Runtime Settings → Task association**:

- **Automatic (last 24 hours)**: Read recent task background.
- **Selected conversations**: Choose at least one historical conversation. Enable **Also read recent tasks** if needed.
- **Off**: Do not automatically associate this task background.

Use [Memory Management](../memory/overview.md) to organize, edit, or export long-term memories. Scheduled tasks can reuse settings through presets; see [Scheduled Tasks](../scheduler/overview.md).

## Notes

- Check the workspace directory before starting a task to avoid writing results to the wrong location.
- To read another directory without changing the main workspace, use a [workspace reference](./attachments.md#workspace-reference).
- Use the actions offered on each file-change card to determine whether a change can be undone. Keep backups of important files. See [Agent Overview](./overview.md).
