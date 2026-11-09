# Preparation Module Guide

> v1.1.11
> 2026-11

Use **Self Improvement → Training → Prepare** to create an SFT (supervised fine-tuning) or pretraining task, configure its model, data, and output location, and start training.

## Contents

- [Before you start](#before-you-start)
- [Interface overview](#interface-overview)
- [Training workflow](#training-workflow)
- [Task management](#task-management)
- [Parameter templates](#parameter-templates)
- [Parameter configuration](#parameter-configuration)
- [Data configuration parameters](#data-configuration-parameters)
- [Training configuration parameters](#training-configuration-parameters)
- [LoRA configuration parameters](#lora-configuration-parameters)
- [Optimization configuration parameters](#optimization-configuration-parameters)
- [Logging configuration parameters](#logging-configuration-parameters)
- [General configuration parameters](#general-configuration-parameters)

## Before you start

- Install an environment with training support under [Settings → System](../settings/environment.md).
- Prepare a complete local model directory with its configuration, weights, and usable tokenizer files. Use a trainable Hugging Face model, not a GGUF file. Supported model weights may use Safetensors or PyTorch format; the weight file does not have to be named `model.safetensors`.
- Prepare training files that match the training type and model tokenizer. See [Data configuration parameters](#data-configuration-parameters). Evaluation during training also requires a separate evaluation file.
- Choose a writable output directory with room for checkpoints. Training uses memory or GPU memory continuously.

Training supports CPU and NVIDIA CUDA. **Auto** uses CUDA when available and otherwise uses CPU. When choosing GPU, also check that the actual environment has CUDA. Other inference backends are not automatically available for training.

## Interface overview

| Area | Purpose |
|------|---------|
| Basic configuration | Task name, parameter template, training type, device, and task selection |
| Operation controls | Open training parameters, start or stop training, and reset the form |

## Training workflow

### Create a new task

1. Open **Self Improvement → Training → Prepare** and choose **New Task** in task management.
2. Keep the generated name or enter a name that identifies the experiment.
3. Select the training type, a matching parameter template, and the device. The available training types are SFT and pretraining, not DPO.
4. Open **Parameter configuration** and set the training data file, model directory, and output directory. The file button beside the model path selects a file inside the model directory and fills in its parent directory.
5. Adjust parameters as needed and click **Apply** at the bottom of the drawer. For a first run, start from a template and use a small dataset to check the workflow.
6. Click **Start**. If a parameter, file, or environment check fails, address the reported issue and try again.
7. Once training starts, the app switches to [Monitor](./monitor.md). Check that the task is running and that logs and metrics appear.

A **Completed** status means the training run has ended. Use [Evaluate](./evaluate.md) and [Test](./test.md) to check whether the result meets your needs.

### Restart an existing task

Select the task in task management, inspect its loaded parameters, apply any changes, and click **Start**. **Selecting an existing task does not resume its previous progress automatically.** To resume, configure a checkpoint as described below.

### Resume from a checkpoint

1. Open the optional settings under **Parameter configuration → Data** and set the checkpoint path for resuming training.
2. Select a checkpoint directory that matches the original model and training configuration and contains the full training state. Final model weights or LoRA weights alone may not be sufficient to restore progress.
3. Check the output directory and other settings, click **Apply**, then **Start**. Check the logs for the checkpoint and step actually restored.

### Environment checks

Starting a task checks training support and required files. Passing these checks does not guarantee sufficient memory, GPU memory, or model compatibility. Review the startup logs as well.

To interrupt training, return to **Prepare**, click **Stop Training**, and wait for the task status to change. Before restarting, check that a usable checkpoint was saved.

## Task management

### Generated task names

The default name combines the training type and local system time, for example `sft_20261109_143052`. Use distinct names for different data or parameter experiments.

### Task list actions

- **New Task**: configure a new training task.
- **Existing task**: load its parameters and status.
- **Delete task**: click the delete icon beside a task that is not running and confirm. This removes its task, training-log, and checkpoint records and cannot be undone in the interface. It does not clean up model output files on disk.

Stop a running task before deleting it.

### Task statuses

The list distinguishes pending, running, completed, stopped, and failed/error states. If a run fails, read the error rather than relying on the progress percentage alone.

## Parameter templates

### Built-in templates

The SFT template enables LoRA by default. The pretraining template does not use LoRA. Templates are starting points, not settings guaranteed to suit every model or device.

### Custom templates

1. Open training parameter configuration, select a template, and edit its values.
2. Click the save icon beside the template selector. Enter a unique name, training type, and optional description, then confirm.
3. Select the saved template on later runs. Non-default templates can be deleted using their delete icon in the drawer's template list, followed by confirmation.

### Temporary parameters

Click **Apply** to use edited values for the current task without overwriting the original template. The page shows a temporary configuration. Closing or cancelling the drawer discards unapplied edits. Save a custom template when you want to reuse the settings.

## Parameter configuration

Parameters are grouped into data, training, LoRA, optimization, logging, and general settings. Some appear in collapsed optional, advanced, or saving groups. The selected template determines which settings are visible.

The following sections explain their main uses. Check the current template and interface for defaults and allowed values. A template's values are not hardware requirements.

## Data configuration parameters

### Training data file path

Prefer `train/final_dataset.jsonl` from [Data Processing](../dataset/sft.md), and the matching `eval/final_dataset.jsonl` for evaluation during training. These are under the processing result's `final` directory; check the output manifest for actual locations. Do not confuse `data.jsonl`, which holds raw samples, with the tokenized training file `final_dataset.jsonl`.

Existing prepared `.jsonl`, `.npy`, and `.mmap` files are also accepted when they match the model tokenizer. Keep companion files alongside a `.mmap` file.

SFT also accepts JSONL with one JSON object per line containing a `messages` array, for example:

```json
{"messages":[{"role":"user","content":"Hello"},{"role":"assistant","content":"Hello!"}]}
```

Raw TXT, ordinary JSON arrays, pretraining records containing only a `text` field, and DPO pairs cannot be used directly as prepared training files here. Pretraining text must first be tokenized and exported in a training format; renaming the extension is not enough.

### Evaluation data file

Select a separate evaluation file and enable an evaluation strategy in the training settings. Its format and tokenizer must match the training data.

**Evaluation sampling ratio** samples the supplied evaluation file; it does not split evaluation data out of the training file. For JSONL/NPY files, `0` or `1` uses all evaluation samples, while a value between them uses that proportion.

### Other data settings

| Parameter | Purpose and notes |
|-----------|-------------------|
| Training model directory | A complete local base model, not a single GGUF file |
| Training output directory | Stores results and checkpoints; use separate directories for separate experiments |
| Tokenizer path | Optional; leave empty to use the model directory's tokenizer |
| Resume checkpoint path | Restores saved training state; see [Resume from a checkpoint](#resume-from-a-checkpoint) |
| Load existing LoRA weights | Not the same as restoring the full training state; leave off for a first run |
| Maximum samples | Limits training samples; `0` means no limit |
| Reshuffle data | Affects sample order; keep consistent in comparisons |
| Data loader workers | Controls loading parallelism; increasing it also uses more resources |
| Pin memory | A data-loading option to adjust for the device and available memory |

## Training configuration parameters

| Parameter | Purpose |
|-----------|---------|
| Batch size | Samples processed together; try lowering it when GPU memory is insufficient |
| Gradient accumulation steps | Accumulates several batches before updating parameters |
| Maximum sequence length | Limits tokens processed per training sample; longer sequences need more resources |
| Learning rate | Controls the size of parameter updates |
| Optimizer and learning-rate scheduler | Control the update method and learning-rate changes; some choices need extra dependencies or supported hardware |
| Training epochs | Number of passes through the data |
| Maximum training steps | A positive value limits update steps; `-1` removes this limit, but epochs still apply |
| Weight decay and maximum gradient norm | Constrain parameter updates |
| Warmup ratio and steps | Control learning-rate warmup at the start |
| Cosine annealing cycles | Used with the corresponding learning-rate scheduler |

### Saving and evaluation during training

- **Save strategy**: `steps` saves at a step interval, `epoch` saves at epoch boundaries, and `no` disables periodic saving. Set **Save steps** when using `steps`.
- **Maximum saved checkpoints**: limits retained checkpoints; older checkpoints may be removed. Back up any checkpoint you need to keep.
- **Evaluation strategy**: defaults to `no`. Selecting `steps` or `epoch` also requires a valid evaluation file. Set **Evaluation steps** when using `steps`.
- **Early stopping patience**: counts consecutive evaluations without improvement, not training steps. `0` disables early stopping. It requires evaluation data and an active evaluation strategy. Match saving and evaluation strategies; for step-based saving, use a save interval that is a multiple of the evaluation interval.
- **Early stopping threshold**: improvements smaller than this value count as no improvement.

The monitor's evaluation-loss curve comes from evaluation during training. It is separate from the benchmark tests on the [Evaluate page](./evaluate.md).

## LoRA configuration parameters

LoRA trains a smaller set of additional weights, usually using fewer resources than updating the whole model. SFT templates provide these options:

| Parameter | Purpose |
|-----------|---------|
| Use LoRA | Enables LoRA; disabling it changes the training method and resource needs |
| LoRA rank (r) | Controls the size of the additional weights |
| LoRA alpha | Controls the scaling of LoRA updates |
| LoRA dropout | Dropout rate during training |
| LoRA target modules | Must match actual layer names in the model; names differ between models |
| Merge LoRA after training | Merges the trained LoRA with the base model and needs additional storage |
| PEFT configuration path | Optional existing LoRA configuration; set only when needed |

Keep the original base model. Testing or evaluating an unmerged LoRA checkpoint still requires it.

## Optimization configuration parameters

- **QLoRA and 8-bit optimizers** require suitable NVIDIA CUDA support and components. Do not enable them solely because GPU memory is low.
- **FP16 and BF16 mixed precision** depend on device support. Do not enable both. CPU training does not use FP16.
- **TF32** is for NVIDIA GPUs that support it.
- **Gradient checkpointing** saves memory through extra computation and may slow training.
- **Low CPU memory usage** reduces memory use while loading a model; training still needs sufficient memory.

## Logging configuration parameters

**Log level** controls verbosity. **Logging steps** controls the interval between recorded training metrics, with a preset value of `10`. Smaller intervals give denser curves and more logs.

## General configuration parameters

**Random seed** controls random processes. Keep the seed, data, and parameters consistent when comparing experiments. Different devices or environments may still produce different results.

## Related guides

- [Training monitoring](./monitor.md): progress, curves, and logs
- [Training evaluation](./evaluate.md): compare checkpoints and the base model
- [Interactive testing](./test.md): inspect actual answers
- [Model quantization](./package.md): export GGUF
