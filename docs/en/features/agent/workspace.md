# Workspace Browser

> v1.1.10
> 2026-10

The workspace browser shows files the agent has read or written in the current workspace, and keeps notes and earlier context available across sessions. File operations stay inside the workspace directory by default. To attach another directory as read-only, see [Attachments & Knowledge Bases](./attachments.md).

## Table of Contents

- [Resource Preview](#resource-preview)
- [Tabs](#tabs)
- [Work Notes](#work-notes)
- [Task Linking](#task-linking)
- [Notes](#notes)

---

## Resource Preview

The workspace includes a resource previewer. Select a file to view it in the app instead of opening an external program first.

Preview covers:

- Ordinary text and code files
- **JSONL** files
- **Live HTML preview**: HTML files render directly in the previewer

The size limit for previewed files has been raised. Above that limit, the previewer warns that it cannot show the full file, or it shows only part of the content. The exact byte limit is the one shown in the interface.

---

## Tabs

The previewer opens files in tabs. Tabs offer more than one close action, so you can close the current tab or a group of related tabs.

The workbench parameter window can be resized horizontally, which leaves room for the preview and the parameters at the same time.

---

## Work Notes

Work notes are shared across sessions. Use them for paths, conclusions, or follow-ups that should not stay inside a single conversation.

To keep a conclusion as an exportable memory, use the feedback actions on a reply, or edit entries in [Memory Management](../memory/overview.md).

---

## Task Linking

Workbench parameters can link the current task to earlier sessions. After a link is set, the agent can read that historical context and continue unfinished work or check a previous decision.

To reuse the same workbench parameters from a schedule, select a parameter preset when you create the scheduled task. See [Scheduled Tasks](../scheduler/overview.md).

---

## Notes

- If no workspace is set, the app uses a default workspace under the temporary directory. Change it in the session configuration.
- An external workspace reference is read-only. The agent cannot write into that directory.
- Previewing HTML or JSONL does not require a separate model load.
- Files the agent changed can still be **rolled back** from the progress node. See [Agent Overview](./overview.md).
