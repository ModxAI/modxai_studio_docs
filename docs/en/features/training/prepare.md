# Prepare Module Details

> v1.0.0
> 2025-12

This article details the functions and parameter configurations of the **Prepare** page in the ModxAI Studio training module, helping you correctly set up training tasks.

## Table of Contents

- [Interface Overview](#interface-overview)
- [Task Management](#task-management)
- [Parameter Configuration](#parameter-configuration)
- [Data Configuration Parameters](#data-configuration-parameters)
- [Training Configuration Parameters](#training-configuration-parameters)
- [LoRA Configuration Parameters](#lora-configuration-parameters)
- [Optimization Configuration Parameters](#optimization-configuration-parameters)
- [Log Configuration Parameters](#log-configuration-parameters)
- [General Configuration Parameters](#general-configuration-parameters)
- [Parameter Templates](#parameter-templates)
- [Training Flow](#training-flow)

---

## Interface Overview

The Prepare module interface is divided into left and right areas:

| Area | Function |
|------|------|
| Left - Basic Config | Task name, config template, training type, training device, task management |
| Right - Operation Control | Parameter configuration entry, start/stop training buttons, reset button |

---

## Task Management

### Task Name Auto-Generation

When creating a new task, the system automatically generates a task name in the format:

```
{training_type}_{timestamp}
```

**Example**: `sft_20241201_143052`

- Training Type: `sft` or `pretrain`
- Timestamp: `YYYYMMDD_HHmmss` format

You can manually modify the task name.

### Task List Operations

| Operation | Description |
|------|------|
| New Task | Select "New Task" option to create a brand new training task |
| Select Existing Task | Select a created task from the dropdown list to load its parameter configuration |
| Delete Task | Click the delete icon to the right of the task to delete non-running tasks |

> ⚠️ **Note**: Running tasks cannot be deleted; training must be stopped first.

### Task Status

The task list displays status tags for each task:

| Status | Color | Description |
|------|------|------|
| Pending | Blue | Task created, waiting to start |
| Running | Green | Training is in progress |
| Completed | Green | Training completed normally |
| Stopped | Orange | Stopped by user |
| Failed | Red | Training exited abnormally |

---

## Parameter Configuration

Click the "Config Params" button to open the parameter configuration drawer. Parameters are classified into six configuration areas by function:

| Configuration Area | English Name | Description |
|----------|--------|------|
| Data Config | data | Training data, model path, output directory, etc. |
| Training Config | training | Batch size, learning rate, save strategy, etc. |
| LoRA Config | lora | LoRA related parameters (Visible for SFT) |
| Optimization Config | optimization | Mixed precision, gradient checkpointing, etc. |
| Log Config | logging | Log level, recording frequency |
| General Config | general | Random seed, etc. |

### Parameter Folding

Parameters are displayed in layers according to importance:

| Fold Group | Description |
|--------|------|
| General Params | Core necessary parameters, expanded by default |
| Optional Params | Not required but commonly used parameters |
| Advanced Params | Advanced tuning parameters |
| Optimization Params | Performance optimization related |
| Save Params | Checkpoint save strategy |
| Evaluation Params | Evaluation strategy during training |

---

## Data Configuration Parameters

### Training data file path

| Attribute | Value |
|------|-----|
| Type | File Path |
| Required | ✅ Yes |
| Default | - |

**Description**: Specify the full path of the training data file.

**Data Format Requirements**:
- **SFT Training**: JSONL format, one JSON object per line, containing dialogue data.
- **Pre-training**: Plain text or JSONL format.

**SFT Data Example**:
```json
{"messages": [{"role": "user", "content": "Hello"}, {"role": "assistant", "content": "Hello! How can I help you?"}]}
{"messages": [{"role": "user", "content": "Introduce yourself"}, {"role": "assistant", "content": "I am an AI assistant..."}]}
```

---

### (opt.) Evaluation data file path

| Attribute | Value |
|------|-----|
| Type | File Path |
| Required | ❌ No |
| Default | - |
| Fold Group | Optional Params |

**Description**: Specify the evaluation data file path for model evaluation during training.

**Usage Scenarios**:
- Used with "Evaluation Strategy" parameter.
- Monitor model performance during training.
- Support early stopping mechanism.

**If evaluation file is not provided**:
- You can use "Evaluation sampling ratio" to automatically split from the training set.
- Or set "Evaluation Strategy" to `no` to skip evaluation during training.

---

### Training output directory

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ✅ Yes |
| Default | - |

**Description**: Specify the output directory for training artifacts.

**Output Content Includes**:
- `checkpoint-{step}/` - Training checkpoint directory
- Final model after training completion

**Notes**:
- Ensure the directory has sufficient disk space.
- Checkpoint quantity × model size may consume a lot of space.
- Can be limited by "Max saved checkpoints" parameter.

---

### Training model path

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ✅ Yes |
| Default | - |

**Description**: Specify the base model directory used for training.

> ⚠️ **Important Reminder**: This parameter requires a **trainable model**, not a GGUF model for inference!

**Correct Model Format**:
- HuggingFace format model directory
- Contains `.safetensors` weight files
- Contains `config.json` configuration file

**Model Directory Structure Example**:
```
model_directory/
├── config.json              # Model configuration (Required)
├── model.safetensors        # Model weights (Required)
├── tokenizer.json           # Tokenizer configuration
├── tokenizer_config.json    # Tokenizer configuration
├── special_tokens_map.json  # Special token mapping
└── generation_config.json   # Generation configuration
```

**Error Examples**:
- ❌ `model.gguf` - GGUF format is only for inference, not trainable.
- ❌ `model.bin` - Old PyTorch format, safetensors is recommended.

**Get Trainable Model**:
1. Download complete model directory from HuggingFace Hub.
2. Select training model category from downloader and download to local directory. Note that this model will not be automatically imported into the Model Library.

---

### (opt.) Tokenizer path

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ❌ No |
| Default | - |
| Fold Group | Optional Params |

**Description**: Specify custom tokenizer directory path.

**Usage Scenarios**:
- Use a tokenizer different from the model.
- Use a custom extended tokenizer.
- Tokenizer files are missing in the model directory.

**Leave Blank**: Automatically use the tokenizer in the model directory.

---

### (opt.) Checkpoint path for resume

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ❌ No |
| Default | - |
| Fold Group | Optional Params |

**Description**: Continue training from the specified checkpoint.

**Resume Training Flow**:

1. **Find Checkpoint**: Find the checkpoint directory to resume from in the output directory, e.g., `checkpoint-500`.

2. **Fill Path**: Fill the full checkpoint path into this parameter.
   ```
   /path/to/output_dir/checkpoint-500
   ```

3. **Start Training**: Click "Start Training", training will continue from that checkpoint.

**Notes**:
- Checkpoint directory must contain complete training state files.
- Step count will continue from previous count after resume.
- To start from scratch, please clear this parameter.

**Checkpoint Directory Structure**:
```
checkpoint-500/
├── adapter_config.json      # LoRA configuration
├── adapter_model.safetensors # LoRA weights
├── optimizer.pt             # Optimizer state
├── scheduler.pt             # Scheduler state
├── trainer_state.json       # Training state
└── training_args.bin        # Training parameters
```

---

### (Not recommended) Attach LoRA weights

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |
| Fold Group | Optional Params |

**Description**: Load existing LoRA weights before training.

> ⚠️ **Not Recommended**: This function is for special scenarios and may cause training instability.

**Usage Scenarios**:
- Continue fine-tuning on existing LoRA basis.
- Multi-stage training process.

---

### Maximum samples

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 0 |
| Range | 0 - 100000 |
| Fold Group | Optimization Params |

**Description**: Limit the number of training samples used.

**Usage Scenarios**:
- Quick test of training flow (set to small value like 100).
- Limit training volume of large datasets.
- Debug parameter configuration.

**Special Value**:
- `0` = Unlimited, use all data.

---

### Evaluation sampling ratio

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 0 |
| Range | 0 - 1 |
| Step | 0.01 |
| Fold Group | Optimization Params |

**Description**: Ratio of evaluation set automatically split from training data.

**Workflow**:
1. Load training data.
2. Randomly split evaluation set according to specified ratio.
3. Remaining data is used for training.

**Example**:
- `0.1` = 10% data for evaluation, 90% for training.
- `0` = No split, need to provide evaluation file separately.

**Usage Suggestions**:
- If independent evaluation file exists, set to `0`.
- If data volume is small (< 1000 items), suggest `0.1` ~ `0.2`.
- If data volume is large (> 10000 items), suggest `0.05` ~ `0.1`.

---

### Reshuffle data

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |
| Fold Group | Optional Params |

**Description**: Whether to reshuffle data order at the beginning of each training epoch.

**Suggestions**:
- For smaller datasets, suggest enabling to increase randomness.
- For very large datasets, can be disabled to save time.

---

### Data loader threads

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 1 |
| Range | 0 - 16 |
| Fold Group | Optimization Params |

**Description**: Number of parallel threads for data loading.

**Tuning Suggestions**:
- Can be appropriately increased when CPU cores are many.
- Windows system suggests keeping 0 or 1.
- Too large may cause increased memory usage.

---

### Use locked page memory

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |
| Fold Group | Optimization Params |

**Description**: Use locked page memory to accelerate data transfer from CPU to GPU.

**Suggestions**:
- Suggest enabling for GPU training.
- Better effect when system memory is sufficient.

---

## Training Configuration Parameters

### Batch size

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 2 |
| Range | 1 - 128 |

**Description**: Number of samples processed per training step.

**Influencing Factors**:
- **VRAM Usage**: Larger batch requires higher VRAM.
- **Training Stability**: Larger batch is usually more stable.
- **Training Speed**: Larger batch may speed up single step training.

**Tuning Suggestions**:

| VRAM | Suggested Batch Size (LoRA) |
|------|----------------------|
| 4GB | 1 - 2 |
| 8GB | 2 - 4 |
| 16GB | 4 - 8 |
| 24GB+ | 8 - 16 |

**When VRAM is insufficient**: Decrease batch size, increase gradient accumulation steps.

---

### Gradient accumulation steps

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 4 |
| Range | 1 - 256 |

**Description**: Update model parameters after accumulating gradients for how many mini-batches.

**Effective Batch Size** = Batch Size × Gradient Accumulation Steps

**Example**:
- Batch Size = 2, Gradient Accumulation = 4 → Effective Batch = 8
- Batch Size = 1, Gradient Accumulation = 8 → Effective Batch = 8

**Usage Scenarios**:
- When VRAM is limited but large batch effect is needed.
- Simulate large batch training with smaller VRAM.

---

### Max sequence length

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 512 |
| Range | 128 - 8192 |
| Step | 8 |

**Description**: Maximum token count of input sequence.

**Impact**:
- Sequences exceeding this length will be truncated.
- Longer sequences require more VRAM.
- Affects context length the model can handle.

**Tuning Suggestions**:
- Set according to actual length of training data.
- Dialogue data usually 512 - 1024 is sufficient.
- Long text tasks may need 2048+.

⚠️ Note: Must match the maximum length in the dataset (affected by the **Max Output Length** parameter in settings) to avoid excessive truncation.

---

### Learning rate

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ✅ Yes |
| Default | 2e-4 |
| Range | 1e-6 - 1e-1 |

**Description**: Step size for model parameter updates.

**Tuning Suggestions**:

| Training Type | Suggested Learning Rate |
|----------|------------|
| LoRA Fine-tuning | 1e-4 ~ 5e-4 |
| Full Fine-tuning | 1e-5 ~ 5e-5 |
| Pre-training | 1e-4 ~ 1e-3 |

**Common Issues**:
- Learning rate too high: Training unstable, loss oscillates or diverges.
- Learning rate too low: Convergence slow, training effect poor.

---

### Optimizer

| Attribute | Value |
|------|-----|
| Type | Select Box |
| Required | ✅ Yes |
| Default | adamw_torch |

**Description**: Select optimization algorithm.

**Common Optimizers**:

| Optimizer | Description |
|--------|------|
| `adamw_torch` | PyTorch native AdamW, recommended |
| `adamw_8bit` | 8-bit AdamW, saves VRAM |
| `paged_adamw_8bit` | Paged 8-bit AdamW, VRAM optimization |
| `adafactor` | High memory efficiency, suitable for large models |
| `sgd` | Stochastic Gradient Descent, simple and stable |

**When VRAM is insufficient**: Try `adamw_8bit` or `paged_adamw_8bit`.

---

### LR scheduler type

| Attribute | Value |
|------|-----|
| Type | Select Box |
| Required | ✅ Yes |
| Default | cosine |

**Description**: Strategy for learning rate change over training process.

**Common Schedulers**:

| Scheduler | Description |
|--------|------|
| `cosine` | Cosine annealing, smooth decay, recommended |
| `linear` | Linear decay |
| `constant` | Constant learning rate |
| `constant_with_warmup` | Constant after warmup |
| `cosine_with_restarts` | Cosine annealing with restarts |

---

### Training epochs

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 2 |
| Range | 1 - 100 |

**Description**: Number of times to completely traverse the training dataset.

**Tuning Suggestions**:
- Small dataset (< 1000 items): 3 - 10 epochs
- Medium dataset (1000 - 10000 items): 2 - 5 epochs
- Large dataset (> 10000 items): 1 - 3 epochs

**Signs of Overfitting**:
- Training loss continues to decrease but validation loss rises.
- Should reduce epochs or enable early stopping at this time.

---

### Weight decay

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 0.01 |
| Range | 0 - 1 |
| Fold Group | Advanced Params |

**Description**: L2 regularization parameter to prevent overfitting.

**Suggestion**: Usually keep default value 0.01.

---

### Warmup ratio

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 0.1 |
| Range | 0 - 1 |
| Fold Group | Optional Params |

**Description**: Proportion of learning rate warmup phase in total training steps.

**Warmup Process**:
- At the beginning of training, learning rate gradually increases from 0 to set value.
- Warmup phase proportion is controlled by this parameter.

**Suggestion**: 0.05 - 0.1 usually works well.

---

### Warmup steps

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 0 |
| Range | 0 - 10000 |
| Fold Group | Optional Params |

**Description**: Specific number of steps for learning rate warmup.

**Priority**: If `warmup_steps > 0` is set, this value is used preferentially, ignoring `warmup_ratio`.

---

### Max gradient norm

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 1.0 |
| Range | 0.1 - 10 |
| Fold Group | Advanced Params |

**Description**: Gradient clipping threshold to prevent gradient explosion.

**Suggestion**: Usually keep default value 1.0.

---

### Max training steps (-1=unlimited)

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | -1 |
| Range | -1 - 100000 |
| Fold Group | Optional Params |

**Description**: Limit maximum training steps.

**Priority**: If `max_steps > 0` is set, it takes precedence over `num_train_epochs`.

**Usage Scenarios**:
- Precisely control training duration.
- Quick test (set to small value like 100).

---

### LR cosine annealing cycles

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 2 |
| Range | 1 - 10 |
| Fold Group | Advanced Params |

**Description**: Number of restart cycles when using `cosine_with_restarts` scheduler.

---

### Save strategy

| Attribute | Value |
|------|-----|
| Type | Select Box |
| Required | ❌ No |
| Default | steps |
| Options | steps / epoch / no |
| Fold Group | Save Params |

**Description**: Trigger strategy for checkpoint saving.

| Strategy | Description |
|------|------|
| `steps` | Save every specified number of steps |
| `epoch` | Save at the end of each training epoch |
| `no` | Do not save intermediate checkpoints |

> ⚠️ **Quota Consumption**: Saving each checkpoint consumes 1 training quota.

---

### Save steps

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 100 |
| Range | 1 - 10000 |
| Fold Group | Save Params |

**Description**: Save checkpoint every X steps when save strategy is `steps`.

**Quota Estimation**: Total Steps ÷ Save Steps = Estimated Checkpoints = Consumed Quotas

---

### Evaluation strategy

| Attribute | Value |
|------|-----|
| Type | Select Box |
| Required | ❌ No |
| Default | no |
| Options | steps / epoch / no |
| Fold Group | Evaluation Params |

**Description**: Strategy for evaluating model during training.

**Prerequisite**: Need to provide evaluation data (evaluation file or evaluation sampling ratio).

| Strategy | Description |
|------|------|
| `steps` | Evaluate every specified number of steps |
| `epoch` | Evaluate at the end of each epoch |
| `no` | Do not evaluate during training |

---

### Evaluation steps

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 100 |
| Range | 1 - 10000 |
| Fold Group | Evaluation Params |

**Description**: Evaluate every X steps when evaluation strategy is `steps`.

---

### Max saved checkpoints

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 100 |
| Range | 1 - 1000 |
| Fold Group | Save Params |

**Description**: Maximum number of checkpoints to retain, automatically delete oldest checkpoints after exceeding.

**Suggestion**: Set reasonable value according to disk space to avoid space exhaustion.

---

### Save interval (minutes)

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 30 |
| Range | 1 - 1440 |
| Fold Group | Save Params |

**Description**: Save checkpoints by time interval in minutes.

---

### Early stopping patience

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ❌ No |
| Default | 0 |
| Range | 0 - 100 |
| Fold Group | Optional Params |

**Description**: Stop training early if evaluation metrics do not improve for consecutive X evaluations.

**Prerequisites**:
- Need to enable evaluation strategy (`eval_strategy` is not `no`).
- Need to provide evaluation data.

**Special Value**:
- `0` = Disable early stopping.

---

### Early stopping threshold

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 0.005 |
| Range | 0.001 - 1 |
| Fold Group | Optional Params |

**Description**: Minimum threshold for evaluation metric improvement, less than this value is considered no improvement.

---

## LoRA Configuration Parameters

> 📝 **Note**: LoRA configuration is only visible under SFT training type, hidden in Pre-training mode.

### Use LoRA fine-tuning

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ✅ Yes |
| Default | true |

**Description**: Whether to enable LoRA (Low-Rank Adaptation) efficient fine-tuning.

**LoRA Advantages**:
- Significantly reduce trainable parameters.
- Significantly reduce VRAM requirements.
- Faster training speed.
- Easy to save and distribute.

**Suggestion**: Strongly recommended to enable for SFT training.

---

### LoRA r parameter

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 16 |
| Range | 1 - 512 |

**Description**: LoRA rank, determines the dimension of low-rank matrices.

**Impact**:
- Larger r means more learnable parameters and stronger expression capability.
- Larger r means higher VRAM and computation overhead.

**Tuning Suggestions**:

| r Value | Applicable Scenario |
|------|----------|
| 4 - 8 | Simple tasks, limited VRAM |
| 16 | General tasks, recommended starting point |
| 32 - 64 | Complex tasks, sufficient VRAM |
| 128+ | Special needs |

---

### LoRA alpha parameter

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 16 |
| Range | 1 - 512 |

**Description**: LoRA scaling factor.

**Scaling Formula**: Actual Scaling = alpha / r

**Common Configurations**:
- `alpha = r`: Scaling factor is 1 (Recommended).
- `alpha = 2r`: Scaling factor is 2, enhance LoRA influence.

---

### LoRA dropout

| Attribute | Value |
|------|-----|
| Type | Float |
| Required | ❌ No |
| Default | 0.0 |
| Range | 0 - 1 |
| Fold Group | Advanced Params |

**Description**: Dropout ratio of LoRA layers, used for regularization.

**Suggestions**:
- Can keep 0 when data volume is large.
- Set to 0.05 - 0.1 when data volume is small or prone to overfitting.

---

### LoRA target modules

| Attribute | Value |
|------|-----|
| Type | Multi-select Box |
| Required | ❌ No |
| Default | ["q_proj", "v_proj"] |
| Fold Group | Advanced Params |

**Description**: Select model modules to apply LoRA.

**Optional Modules**:

| Module | Description |
|------|------|
| `q_proj` | Query Projection Layer (Recommended) |
| `k_proj` | Key Projection Layer |
| `v_proj` | Value Projection Layer (Recommended) |
| `o_proj` | Output Projection Layer |
| `gate_proj` | Gate Projection Layer |
| `up_proj` | Up Projection Layer |
| `down_proj` | Down Projection Layer |
| `embed_tokens` | Embedding Layer |
| `lm_head` | Language Model Head |

**Tuning Suggestions**:
- Basic Config: `q_proj`, `v_proj`
- Enhanced Config: Add `k_proj`, `o_proj`
- Full Config: Add `gate_proj`, `up_proj`, `down_proj`

---

### Merge LoRA weights after training

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |
| Fold Group | Optional Params |

**Description**: Whether to automatically merge LoRA weights into base model after training completion.

**After Merge**:
- Generate complete model weight files.
- No longer need to load LoRA adapter.
- File size is same as original model.

**Not Merge**:
- Only save LoRA adapter weights (small size).
- Need to load both base model and LoRA during inference.

---

### PEFT config path

| Attribute | Value |
|------|-----|
| Type | File Path |
| Required | ❌ No |
| Default | - |
| Fold Group | Optional Params |

**Description**: Specify custom PEFT configuration file path.

**Usage Scenarios**: Use predefined PEFT configuration instead of interface parameters.

---

## Optimization Configuration Parameters

### Use FP16 mixed precision

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ✅ Yes |
| Default | true |

**Description**: Use FP16 half-precision floating point for mixed precision training.

**Advantages**:
- Reduce VRAM usage by about 50%.
- Accelerate training process.
- Maintain training precision.

**Suggestion**: Recommended to enable for GPU training.

---

### Use BF16 mixed precision (NVIDIA GPU)

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |

**Description**: Use BF16 format for mixed precision training.

**Requirements**:
- NVIDIA Ampere and above architecture GPU (RTX 30 series, A100, etc.).
- Choose one between FP16.

**Advantages**:
- Larger numerical range, more stable training.
- Suitable for large model training.

---

### Use TF32 (NVIDIA GPU)

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ❌ No |
| Default | false |

**Description**: Use TensorFloat-32 format to accelerate matrix operations.

**Requirements**: NVIDIA Ampere and above architecture GPU.

---

### Gradient checkpointing

| Attribute | Value |
|------|-----|
| Type | Boolean |
| Required | ✅ Yes |
| Default | true |

**Description**: Trade computation time for VRAM space.

**Working Principle**:
- Do not save intermediate activation values during training.
- Recompute during backpropagation.
- Significantly reduce VRAM usage.

**Suggestion**: Strongly recommended to enable when VRAM is limited.

---

## Log Configuration Parameters

### Log level

| Attribute | Value |
|------|-----|
| Type | Select Box |
| Required | ✅ Yes |
| Default | info |
| Options | debug / info / warning / error |

**Description**: Detail level of training logs.

| Level | Description |
|------|------|
| `debug` | Most detailed, includes debug info |
| `info` | Regular info, recommended |
| `warning` | Only warnings and errors |
| `error` | Only error info |

---

### Logging steps

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 10 |
| Range | 1 - 1000 |

**Description**: Record training logs every X steps.

**Impact**:
- Smaller value means more frequent monitor interface updates.
- Too small value may affect training efficiency.

---

## General Configuration Parameters

### Random seed

| Attribute | Value |
|------|-----|
| Type | Integer |
| Required | ✅ Yes |
| Default | 42 |
| Range | 0 - 999999 |

**Description**: Control randomness during training.

**Function**:
- Same seed can reproduce training results.
- Convenient for comparative experiments.

---

## Parameter Templates

### System Preset Templates

The system provides two default templates:

| Template | Description |
|------|------|
| SFT | Supervised fine-tuning default configuration, LoRA enabled |
| Pretrain | Pre-training default configuration, LoRA disabled |

### Custom Templates

1. **Save Template**: After configuring parameters, click "Save As" to save as a custom template.
2. **Load Template**: Select saved template from template dropdown box.
3. **Delete Template**: Click delete icon to the right of template in dropdown box.

### Temporary Parameters

When checking "Temporary Template" option during task creation:

- **Do not overwrite original template**: Currently modified parameters will not be saved to the currently selected template.
- **Save as task parameters only**: Parameters will be saved as independent configuration for this task.
- **Backend reads task parameters**: Backend uses current temporary parameters during training, irrelevant to template.
- **Support subsequent save as**: If this set of temporary parameters works well, you can save it as a new template later.

---

## Training Flow

### Create New Task

1. Select "New Task" in task management.
2. Set task name (or use auto-generated name).
3. Select training type (SFT or Pre-training).
4. Select configuration template or use custom configuration.
5. Click "Config Params" to adjust various parameters.
6. Confirm required parameters are filled completely.
7. Click "Start Training".

### Continue Existing Task

1. Select existing task in task management.
2. System automatically loads parameter configuration of that task.
3. Can adjust parameters as needed.
4. Click "Start Training".

### Resume Training

1. Fill checkpoint directory path in "Checkpoint path for resume".
2. Ensure checkpoint contains complete training state.
3. Click "Start Training", will continue from that checkpoint.

### Environment Check

Before starting training, system will automatically check:
- Whether Python training environment is ready.
- Whether necessary dependency packages are installed.
- Whether GPU/CUDA is available (if GPU training is selected).

---

## Next Steps

- [Overview](./overview.md) - Training function overview
- [Monitor Module Details](./monitor.md) - Real-time monitoring and history review
- [Evaluate Module Details](./evaluate.md) - Checkpoint evaluation and comparison
- [Test Module Details](./test.md) - Interactive testing
- [Package Module Details](./package.md) - GGUF export
