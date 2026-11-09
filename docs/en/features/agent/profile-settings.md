# Instruction Settings

> v1.1.11
> 2026-11

Instruction settings allow configuring global instruction files and personal preferences, so the agent follows your customized requirements on every execution.

## Table of Contents

- [Global Instruction File](#global-instruction-file)
- [Personal Preferences](#personal-preferences)
- [Workspace Auto Detection](#workspace-auto-detection)
- [Search Ignore File](#search-ignore-file)

---

## Global Instruction File

An instruction file is a plain text file whose content is injected into the agent's context as system instructions, influencing the agent's behavior.

### Configuration

Open **Settings → Agent** to find **Global Instruction File** and **User Preferences**. Set the workspace directory in the agent page’s **Runtime Settings**.

#### Method 1: Auto Detection (Recommended)

1. Set the **Workspace Directory**.
2. The system automatically detects `AGENTS.md`-like files in the workspace root.
3. The file takes effect automatically once detected.

#### Method 2: Manual Specification

1. Click **Browse Instruction File**.
2. Select a plain text file.
3. Click **Save** at the bottom of the page and check the active path shown under the file field.

### Instruction File Format

- Plain text file (`.md`, `.txt`, etc.)
- Content is injected as part of the system prompt.
- Recommended to include project rules, behavior constraints, output format, etc.

An instruction file detected in the workspace takes priority over a manually selected file. Check the active path shown on the page.

---

## Personal Preferences

Personal preferences allow you to add multiple short descriptions to remind the agent of your habits and preferences.

### Adding Preferences

1. Open **Settings → Agent** and find **User Preferences**.
2. Click **Add Preference**.
3. Enter a description (200 characters or fewer per entry).
4. Add more preferences if needed, then click **Save**.

### Preference Examples

- "Always respond in English."
- "Keep responses concise."

### Managing Preferences

- Click the delete button next to an entry, then click **Save**.
- All preferences are merged into the agent's system prompt with higher weight.

---

## Workspace Auto Detection

After setting the workspace directory, the system automatically detects instruction files at the following locations:

- `AGENTS.md` in the workspace root
- `AGENTS.txt` in the workspace root
- `agents.md` in the workspace root

The app checks these names in the order listed and uses the first file it finds. Check the active path shown on the page.

## Search Ignore File

Under **Settings → Agent → Global Instruction File**, click **Browse Ignore File**, choose a text rules file, and click **Save**. A `.modxignore` file in the workspace root takes priority over a manually selected file. Without a selected file, the app uses its built-in ignore rules. These rules affect file searches and do not replace tool permissions or approval.
