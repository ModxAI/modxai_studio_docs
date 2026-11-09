# Model Training

> v1.1.11
> 2026-11

Prepare a model and dataset, create a training task, monitor it, then evaluate, test, and export the result. Open **Self Improvement → Training** from the main menu.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Functional Modules](#functional-modules)
- [Training Task Mechanism](#training-task-mechanism)
- [Training Parameter System](#training-parameter-system)
- [Parameter Template Mechanism](#parameter-template-mechanism)
- [Next Steps](#next-steps)

## Environment Requirements

Install an environment with training support in [Settings → System](../settings/environment.md). Training supports CPU or NVIDIA CUDA. Memory, GPU memory, and storage requirements depend on the model, training method, sequence length, batch size, and checkpoint settings. There is no single minimum GPU-memory figure for every model.

Prepare a complete trainable model directory with configuration, weights, and a usable tokenizer, not a GGUF inference file. For SFT/pretraining output from Data Processing, prefer `train/final_dataset.jsonl` and select `eval/final_dataset.jsonl` separately when evaluating during training. See [Prepare](./prepare.md).

## Feature Overview

1. In [Prepare](./prepare.md), select SFT or pretraining, enter model, data, and output paths, apply parameters, and click **Start**.
2. In [Monitor](./monitor.md), confirm the task is running and inspect loss, speed, and logs.
3. Once training has ended and checkpoints are available, compare benchmarks in [Evaluate](./evaluate.md) and inspect your own questions in [Test](./test.md).
4. Choose a checkpoint and follow the [quantization guide](./package.md) to export GGUF.

Data Processing can export DPO preference data, but this training interface has no DPO training type.

## Interface Layout

| Tab | Purpose |
|-----|---------|
| Prepare | Create tasks, configure parameters, and start or stop training |
| Monitor | Inspect current progress and saved metric history |
| Evaluate | Compare the base model and checkpoints on fixed benchmarks |
| Test | Enter questions and inspect the selected checkpoint's answers |
| Quantize | Export supported training outputs to GGUF |

## Functional Modules

### Prepare Module

Choose the training type, device, and template, then configure training data, model, and output locations. The SFT template enables LoRA by default; the pretraining template does not use it. Settings must still match the model, and templates are not guaranteed to suit every device.

### Monitor Module

Select a running task for live metrics or a task that is not running for saved history. Charts include training loss, perplexity, learning rate, gradient norm, evaluation loss, and speed. Live charts show up to the latest 400 points, and history charts up to 4000, rather than necessarily showing every recorded point.

Evaluation loss during training requires a separate evaluation file and an active strategy. It is different from benchmark evaluation below.

### Evaluate Module

Choose a training task and one or more checkpoints, optionally including the base model. Built-in benchmarks are IFEval, MMLU-Pro, GSM8K, and HellaSwag, with Quick, Extended, and Full scales.

Prepare resources as prompted, click **Check and continue**, then **Start Evaluation** after the checks pass. Results include a score matrix, benchmark trends, individual samples, and two-model comparisons. **Highest for This Benchmark** is a per-benchmark result, not an overall best-model recommendation.

Use **Evaluation History** to view or continue runs, copy settings, recalculate, or export JSONL. The current evaluation page has no custom question-and-answer sample input.

### Test Module

Choose an available checkpoint from a finished task, configure inference parameters and an optional system prompt, and start entering questions. LoRA checkpoints also need their original base model.

Each input is independent and does not include earlier questions and answers as context. Exit before changing parameters or checkpoints.

### Quantize Module

Select model artifacts, an output location, and available quantization options to export GGUF. See the [quantization guide](./package.md) for supported models and full instructions.

## Training Task Mechanism

Tasks show pending, running, completed, stopped, or failed/error states. Completion means the run ended; it does not guarantee correct answers from the trained model.

Checkpoints follow the save strategy, step interval, and retention limit. Older checkpoints may be removed when the limit is exceeded. Back up any checkpoint you need to keep.

Selecting an existing task and clicking **Start** does not automatically resume its previous progress. Set a usable resume checkpoint as described in [Prepare](./prepare.md#resume-from-a-checkpoint).

## Training Parameter System

Settings are grouped into data, training, LoRA, optimization, logging, and general configuration. See [Prepare](./prepare.md#parameter-configuration) for details.

- Required paths: training data, a trainable model directory, and a writable output directory.
- Evaluation during training: supply a separate evaluation file and enable a strategy. Evaluation sampling ratio samples that file; it does not split the training data.
- Resource use: mainly depends on the model size, training method, batch size, and sequence length.
- Early stopping: counts consecutive evaluations without improvement, not training steps.
- Comparisons: keep data, seed, and parameters consistent. Identical results across devices are not guaranteed.

## Parameter Template Mechanism

Load a built-in or custom template, edit it in training parameter configuration, and click **Apply**. Changes apply to the current task without overwriting the original template. To reuse them, click the save icon beside the template selector and create a custom template.

Expand optional parameter groups as needed. Cancelling or closing the drawer does not apply unconfirmed edits.

## Next Steps

- [Prepare](./prepare.md)
- [Monitor](./monitor.md)
- [Evaluate](./evaluate.md)
- [Test](./test.md)
- [Quantization guide](./package.md)
