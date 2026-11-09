# Improve a Skill

> v1.1.11
> 2026-11

A Skill keeps task instructions and supporting material in a directory. RSI can revise an existing Skill or start from a new one, then use an agent to perform tasks and check the result.

## Prerequisites

- Configure a student agent that can perform the tasks.
- Enable tools for the student's main role and allow **skilltool**.
- For an existing Skill, make sure its directory is readable and contains a valid **SKILL.md**.
- Prepare a test workspace and review approvals for tasks that change files or perform external actions.

## Improve an existing Skill

1. Open **Self Improvement → New Task** and set **Improve** to **Skill** or **Both**. The task mode uses **Agent**.
2. Select the teacher model and click the student's **Configure** button (the gear icon).
3. Review the main-role model and tools. Set the Skill root directory containing your existing Skill subdirectories.
4. Return to Configure, click **Refresh Skills** beside **Starting Skill**, and select the Skill to improve.
5. Review **SKILL.md** and the supporting files. You can edit text in the file tabs; these edits affect this draft.
6. Enter the task name, improvement goal, and evaluation tasks, then set the stopping rules.
7. Click **Start**. Use **View Conversation** to handle approvals or additional input in the round's agent conversation.

With **Skill**, the system prompt stays fixed while the Skill text changes. With **Both**, the prompt and Skill are improved as one solution. Check the initial content in each tab.

## Create a Skill

1. Select **New Skill** in **Starting Skill**.
2. Enter a **Directory Name**. It must not be empty, start with a dot, or contain `/`, `\`, or `:`.
3. Edit the generated **SKILL.md** to describe when to use the Skill, what to do, and which constraints to follow. Keep the metadata at the beginning and a non-empty `description`.
4. To add supporting material, enter a file name such as `guide.md` and click **New Reference File** (the plus icon). The file is placed in the `references` directory.
5. Enter its content in the new tab. You can remove other supporting files using the tab's remove button; **SKILL.md** cannot be removed.
6. Add evaluation tasks and start the run.

## What RSI can change

- RSI can edit and revise **SKILL.md** and supported UTF-8 text files: `.md`, `.txt`, `.json`, `.yaml`, `.yml`, `.csv`, and `.tsv`.
- Content in **scripts** and **assets** is not rewritten. Existing scripts and resources are preserved in the Skill copy and included in the export.
- Each round uses a separate Skill copy, so iteration does not directly rewrite the original Skill directory. The agent can still change other files while performing the task; review its workspace and permissions.
- Changing the root directory after selecting a Skill may invalidate that selection. If the source no longer matches, restore the original root or select the Skill again.
- The agent must actually load the target Skill for a valid comparison. If the page says it was not loaded, check the linked conversation and tool settings. The saved answer remains available but is not valid evidence of that Skill's performance.

## Use the result

After reviewing **Compare** and **Rounds**, open **Export** and select the round you want to keep. Export to a new parent directory first and inspect the files before replacing the original Skill.

Exporting does not switch your current agent to the new solution. To use the exported Skill, select its parent directory as the Skill root in Agent settings, refresh the list, and select the Skill. For a combined solution, also apply the exported prompt to the intended role.

See [Compare and Export Results](./results.md) for the steps.
