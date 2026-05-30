# Instruction Settings

> v1.1.0
> 2026-05

Instruction settings allow configuring global instruction files and personal preferences, so the agent follows your customized requirements on every execution.

## Table of Contents

- [Global Instruction File](#global-instruction-file)
- [Personal Preferences](#personal-preferences)
- [Workspace Auto Detection](#workspace-auto-detection)

---

## Global Instruction File

An instruction file is a plain text file whose content is injected into the agent's context as system instructions, influencing the agent's behavior.

### Configuration

Click the **Instruction Settings** button in the right panel to open the instruction settings page.

#### Method 1: Auto Detection (Recommended)

1. Set the **Workspace Directory**.
2. The system automatically detects `AGENTS.md`-like files in the workspace root.
3. The file takes effect automatically once detected.

#### Method 2: Manual Specification

1. Click **Browse Instruction File**.
2. Select a plain text file.

### Instruction File Format

- Plain text file (`.md`, `.txt`, etc.)
- Content is injected as part of the system prompt.
- Recommended to include project rules, behavior constraints, output format, etc.

---

## Personal Preferences

Personal preferences allow you to add multiple short descriptions to remind the agent of your habits and preferences.

### Adding Preferences

1. Find the "Personal Preferences" area on the instruction settings page.
2. Click **Add Preference**.
3. Enter a description (200 characters or fewer per entry).
4. Multiple preferences can be added.

### Preference Examples

- "Always respond in English."
- "Keep responses concise."

### Managing Preferences

- Click the delete button next to a preference entry to remove it.
- All preferences are merged into the agent's system prompt with higher weight.

---

## Workspace Auto Detection

After setting the workspace directory, the system automatically detects instruction files at the following locations:

- `AGENTS.md` in the workspace root
- `.agents.md` in the workspace root
- Other AGENTS-like naming patterns

Detected files take effect automatically and require no manual specification. This is the recommended approach — simply place an `AGENTS.md` file in the workspace root, and all agents running in that workspace will follow its instructions.
