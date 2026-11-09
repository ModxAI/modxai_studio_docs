# Training Test Guide

> v1.1.11
> 2026-11

Use **Self Improvement → Training → Test** to load a checkpoint, enter questions, and inspect its answers on your actual tasks.

## Contents

- [Before you start](#before-you-start)
- [Testing workflow](#testing-workflow)
- [Inference parameters](#inference-parameters)
- [Testing suggestions](#testing-suggestions)
- [Common questions](#common-questions)
- [Related workflow](#related-workflow)

## Before you start

- Install a [runtime environment](../settings/environment.md) with training support.
- Have a finished training task and a usable checkpoint. The selector includes completed, stopped, failed, and error tasks, but a task record does not guarantee a loadable checkpoint.
- Keep the complete model and tokenizer files. Testing an unmerged LoRA checkpoint also requires the original base model directory.
- Leave sufficient memory or GPU memory to load the model.

## Testing workflow

1. Open **Self Improvement → Training → Test** and select a training task.
2. Select a checkpoint. For a LoRA task, check the displayed base model path. If necessary, select a file inside the base model directory to update it. A non-LoRA task uses the selected checkpoint directly.
3. Open **Inference Parameter Configuration**, adjust generation settings and the optional system prompt, and click **Apply**.
4. Click **Start** and wait for environment checks and model loading. The page switches to **Interactive Test**, showing logs and an input field.
5. Enter a question and press Enter or click **Send**. Read the question and answer in the log. Wait for the answer to finish before sending another question.
6. Click **Quit** when finished. Exit before changing the checkpoint or parameters, then configure and start a new test.

**Each input is answered independently. Previous questions and answers are not included as context.** Consecutive answers in the log do not mean the test supports contextual multi-turn conversation. Include any required background in the current question.

## Inference parameters

### Basic parameters

| Parameter | Default | Purpose and limits |
|-----------|---------|--------------------|
| Temperature | 0.7 | Adjusts sampling randomness; start with the default and make small changes |
| Maximum length | 256 | Maximum new tokens in the answer; the interface allows 1–8192. This is not the combined input and output length |
| Top P | 0.9 | Filters candidate tokens by cumulative probability |
| Top K | 50 | Restricts candidates to the most likely tokens; `0` disables this limit |
| Repetition penalty | 1.1 | Discourages repeated tokens; `1` adds no penalty. Avoid `0` |
| Presence penalty | 1.1 | Shown in the interface but does not affect generation in the current interactive test |
| Min P | 0 | Filters candidates relative to the most likely token; `0` disables it |
| Random seed | 42 | Helps repeat tests; `0` leaves it unfixed. The same seed does not guarantee identical output across devices or environments |

Tokens are the model's units of text, not characters. Increasing maximum length takes more time and may need more memory. The model's context capacity still applies.

### Advanced parameters

- **Enable thinking chain**: off by default. It works only when supported by the model and its chat template; it does not add this capability to other models.
- **System prompt**: sets the model's role or answer requirements, up to `4000` characters. For example: “Answer briefly and say when you are unsure.”

Parameters take effect when the test starts. Exit and restart the test to compare different settings.

## Testing suggestions

- Prepare a fixed question set covering common tasks, unfamiliar phrasing, output formats, and questions beyond the model's knowledge.
- Keep questions, system prompt, and generation settings consistent when comparing checkpoints. Record which checkpoint produced each result.
- Change one parameter at a time so you can identify its effect.
- Check facts, formatting, and omissions yourself. Fluent answers or a higher benchmark score do not establish reliability.

## Common questions

### No tasks or checkpoints are available

Check that training has ended and actually saved a checkpoint. Task records may remain even if checkpoint files were moved, deleted, or left incomplete.

### Answers are cut short

Check whether **Maximum length** is too small and try increasing it. The model may also end an answer on its own. When thinking is enabled, allow room for both thinking and the final answer.

### Answers repeat or go off topic

Retry the same question with defaults, then adjust temperature or repetition penalty one at a time. Also review the training data and system prompt. Presence penalty does not change the current test's output.

### Follow-up questions do not use earlier context

This is an independent-question test. Put the required background in the current question instead of sending only a pronoun or a context-dependent follow-up.

### The model fails to load

Check the checkpoint, base model, and tokenizer paths and free memory used by other programs. After a failed test, inspect the retained log and click **Close** to return to configuration before correcting the issue and retrying.

## Related workflow

First compare the base model and checkpoints in [Evaluate](./evaluate.md), then use Test for your own questions. Once you select a checkpoint, continue to [Quantize](./package.md) to export GGUF.

## Related guides

- [Prepare](./prepare.md): create training tasks
- [Monitor](./monitor.md): review training progress
- [Evaluate](./evaluate.md): run benchmark evaluations
- [Quantize](./package.md): export the model
