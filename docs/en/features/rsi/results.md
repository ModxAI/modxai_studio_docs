# Compare and Export Results

> v1.1.11
> 2026-11

Review each round's changes, answers, and evidence, then choose a prompt or Skill to keep. You can use the recommended solution or export another existing round.

## Review the results

1. Open **Self Improvement → Compare**, or open a run from **History**.
2. Select **Baseline** or a round and read its **Optimization Strategy**.
3. For Prompt or Both runs, click **Expand** to compare the baseline prompt with the selected round's prompt.
4. Read the answers in **Comparison**, then the **Key Improvements**, **Remaining Issues**, and **Evaluation Evidence** in the **Evaluation Report**.
5. For multiple tasks, return to **Iterate → Rounds** and inspect each task's answer in each round. For agent tasks, use **View Conversation** to check actual actions and outputs.
6. Verify important facts, calculations, and file content before choosing a version.

## Understand scores and recommendations

- The baseline is the original solution. The teacher defines criteria first, then evaluates the results.
- A round's score is its average task score, from 0–100. Content without a completed, valid evaluation is not a verified solution.
- **Highest Score So Far** in **Score Progression** is not the same as the recommended solution. Recommendations also account for the candidate and reference solution being evaluated together. Do not select a result solely by the chart's highest point.
- The baseline may remain recommended if no better solution was selected. More rounds do not guarantee improvement.
- An excerpt warning means grading used only part of the evidence. If no valid comparison completed, existing content is still available but needs separate verification.
- Scores should not be treated as a direct ranking across changes to models, parameters, tasks, material, or environment.

## Copy or export

Wait for the run to finish if its status is Running or Stopping.

1. Open **Export**.
2. Select the version in the round list. **Compare** and **Export** share the selected round, so check it again after browsing other rounds.
3. Review the content. You can edit the prompt or existing Skill text files before saving.
4. Click **Copy** to copy the current prompt or the active Skill file tab's text.
5. To save files, click **Export** and choose a location in the system dialog.
6. If **Overwrite Existing Files?** appears, review the file list before clicking **Overwrite Files**. If unsure, cancel and choose a new directory.
7. Wait for **Exported**, then inspect the chosen directory.

| Improve | Files saved inside the selected directory |
|---|---|
| Prompt | `prompt.txt` |
| Skill | A subdirectory named after the Skill, containing `SKILL.md`, supporting text, and preserved original resources |
| Both | `prompt.txt` and the Skill subdirectory |

Choose the **parent directory**. For example, exporting `support-guide` into `D:\RSI-exports` creates a `support-guide` subdirectory there. Do not select another `support-guide` subdirectory unless you intend that extra nesting.

## Edits and overwrites

- Edited content shows **Modified**. The original score describes the tested version, not your subsequent edits.
- Export lets you edit or remove existing supporting text files, but not add new ones or rename the Skill. Add supporting files in a configuration draft. Directory names can only be edited for newly created Skills.
- Saving writes the exported files without clearing the target directory. Older files excluded from the new solution may remain when exporting into an existing directory. Prefer an empty directory for a clean version.
- If the page reports that the files or content changed, review the destination and current text, then export again.
- Directory saving requires the desktop app's system folder picker. If it is unavailable, export from the desktop app. You can still copy the current text.

## Use the result elsewhere

- **Prompt**: paste it into the target chat's system prompt or the intended agent role's system prompt settings.
- **Skill**: select the export directory as the Skill root in Agent settings, refresh the list, and select the Skill.
- **Both**: apply both the prompt and the Skill, then test them on real tasks.

A successful export does not switch the current chat or agent automatically. If the page says the files were saved but Skills need refreshing, check the files and refresh the list rather than overwriting them again just because the refresh failed.

Next: [History, Continuation, and Improvement Notes](./history.md)
