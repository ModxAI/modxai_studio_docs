# Evaluation Tasks and Conversation Imports

> v1.1.11
> 2026-11

Evaluation tasks are the questions or actions repeated in each round. An RSI run accepts **1–10 tasks**, entered manually or imported from chat or agent conversations.

## Add a custom task

1. Open **Self Improvement → Configure → Evaluation Tasks** and find **Custom Task**.
2. Enter the question, context, and requirements. If you use a quick-fill example, supply its missing conditions and material.
3. If needed, use the attachment button to choose files and check their names.
4. Click **Add Task**. Text left in the input box is not yet part of the evaluation list.
5. Expand **Input** on the added task to review its messages and attachments. To replace a task, remove it and add a new one.

Choose representative cases, such as a typical question, missing information, and an easy-to-misread boundary case. State the required format, calculation conditions, or source limits instead of only asking for a “better answer.”

## Choose attachments and a task mode

- **Chat** mode offers image and video attachments. Use a model that supports the relevant input.
- Use **Agent** mode for regular files and audio, with the necessary tools or environment configured.
- If an imported task says it requires agent execution because it contains files or audio, change **Task mode** and review the agent settings.
- Select files through the attachment button rather than entering a local path as an attachment. Check that the files remain readable before starting.

Online models may receive attachment content in their requests. Remove material that should not be sent to the provider.

## Import from Chat

1. Click **Import Conversations** and select **Chat**.
2. Select a **Conversation**.
3. Under **Answer**, select the historical answer you want to examine.
4. Expand **Input Preview** and check the preceding question and relevant conversation context.
5. Click **Confirm Import**, then review the task and models in Configure.

The selected answer provides a case for analysis. Each round executes the corresponding input again; the old answer is not supplied as a correct answer for the student to copy.

The first imported chat task may fill in an available system prompt and student model. History does not always contain complete execution parameters. If the page says the original parameters are unavailable, review the model and settings for this run.

## Import from an agent conversation

1. Click **Import Conversations** and select **Agent**.
2. Select a conversation, then a specific **Task Description**.
3. Review **Input Preview** and click **Confirm Import**.
4. First confirm that **Task mode** is **Agent**, then open the student's **Configure** panel and check models, tools, approvals, workspace, and Skills.

If the imported item is the first task in the list, the page may bring in an available agent configuration and change the task mode. If other tasks are already present, check the mode yourself. Later imports do not automatically overwrite settings you have already changed manually. Do not assume each task uses every setting from its source conversation.

An imported task may still execute in its original conversation workspace. Changing the student workspace alone does not move it to a test directory. To use another directory, create the task manually, select a test workspace, and check its input material so the original directory is not changed.

## Supply missing files

When a task shows **Missing files**:

1. Read the listed names.
2. Click the task's attachment button, **Supply Input Files With the Same Names**.
3. Select the matching files and check that the warning clears.
4. If duplicate names make the match ambiguous, add the task again using distinct file names.

You cannot start while required files are missing. If the historical material cannot be recovered, remove the task and recreate it with available attachments.

## Before starting

- Check the task count. Text waiting in the input box has not been added yet.
- Each task should explain what to do, with its files and permissions ready.
- An agent may repeat the same action across rounds. Use test targets and review approvals for tasks that send messages, change data, or call paid services.

Next: [Configure and Start a Run](./configure.md) · [Improve a Skill](./skills.md)
