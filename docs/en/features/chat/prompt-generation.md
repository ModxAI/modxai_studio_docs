# Prompt Optimization

> v1.1.11
> 2026-11

Turn a task description into a prompt, review it, and place it in the chat or agent input. Generating a prompt does not execute the task or send the result automatically.

## Prerequisites

Load a local model suitable for text generation in [Model Center](../models/overview.md), with the runtime environment required by that model. Configuring only an online agent model does not meet this local-model requirement.

## Generate and Use a Prompt

1. Click the magic-wand button beside the chat or agent input to open **Prompt Generation**.
2. Choose a **System Prompt Template** and a user prompt template as needed. Selecting a system template attempts to select its paired user template; you can change the combination.
3. Enter your goal, context, constraints, and desired format in **User Input Content**.
4. Click **Generate** and wait for the text in **Generated Result**.
5. Edit the result directly. Remove any unsupported assumptions or requirements the model added.
6. Click **Apply to Inputbox** to replace the current input text. Review it and send manually. Copy the original draft first if you need to keep it.

The system template defines the rewriting instructions. The user template describes how to process the original input. The system template, user template, and original input are editable, each with a **4,000-character** limit.

**Cancel** closes the window without applying the result. It does not clear the content you entered.

## Custom Templates

### Include the Original Input

Use a single-brace placeholder in the user prompt template, for example:

```text
Rewrite this task clearly. Keep its meaning, do not invent context, and return only the revised prompt:
{userPrompt}
```

During generation, `{userPrompt}` is replaced with **User Input Content**. Other supported placeholders are `{content}`, `{user_content}`, `{query}`, `{originalPrompt}`, `{input}`, and `{text}`.

If a nonempty user template has no placeholder, the original input is not appended automatically. Leaving the user template empty sends the original input directly.

### Save a Template

1. Edit the system or user template, leaving at least one part nonempty.
2. Click **Save Template**.
3. Enter a name with at least two characters, add an optional description, and confirm.

Names cannot contain `\ / : * ? " < > |`. Only nonempty parts are saved, with `_system` and `_user` appended to their names for pairing.

### Delete a Template

Select the template, click its delete button, and confirm. Deletion also removes its matching system/user template pair and cannot be undone. Copy any content you need before deleting.

## FAQ

### Cannot Generate

Check that a local model is loaded, **User Input Content** is not empty, and another generation is not still running. For model and environment issues, see [Loading Models](../models/overview.md).

### Empty or Unsuitable Results

Check any error message and the model status first. If generation completes but the result is unsuitable, adjust the template, add real constraints, and try again. A complete, clear task can be sent directly without optimization.

### Original Input Missing from the Result

Check the placeholder spelling in the user template; use `{userPrompt}`. The placeholder passes the original input to the model but does not guarantee that the result will repeat it word for word.
