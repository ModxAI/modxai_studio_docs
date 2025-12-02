# Model Training

> v1.0.0
> 2025-12

This article introduces how to use the **Model Training** function in ModxAI Studio, helping you complete the full training workflow from data preparation to model output.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Functional Modules](#functional-modules)
- [Training Task Mechanism](#training-task-mechanism)
- [Quota Consumption Rules](#quota-consumption-rules)
- [Training Parameter System](#training-parameter-system)
- [Parameter Template Mechanism](#parameter-template-mechanism)
- [Next Steps](#next-steps)

---

## Environment Requirements

### Basic Requirements

| Requirement | Description |
|------|------|
| Runtime Environment | Need to install GPU or CPU environment in the **Settings** |
| VRAM | LoRA fine-tuning minimum 4GB, recommended 8GB or above |
| Disk Space | Sufficient storage space for checkpoints and logs |

### Environment Installation

On the training preparation page, click "Environment Config" to install Python dependencies required for training:

- **GPU Environment**: Install PyTorch CUDA version and related training libraries.
- **CPU Environment**: Install PyTorch CPU version (training speed is slower).

### Hardware Recommendations

| Model Scale | VRAM Requirement (LoRA) | VRAM Requirement (Full) |
|----------|----------------|----------------|
| Below 1B | 4GB | 16GB |
| 1-3B | 6GB | 24GB |
| 3-7B | 8GB | 40GB |
| 7-14B | 12GB | 80GB |

---

## Feature Overview

The Model Training module is one of the core functions of ModxAI Studio, providing a complete workflow from training configuration to model output. Key features include:

- **Multi-Training Type Support**: Supports both SFT (Supervised Fine-Tuning) and Pre-training.
- **LoRA Efficient Fine-Tuning**: SFT uses LoRA technology by default, significantly reducing VRAM requirements.
- **Real-time Monitoring**: Visual display of core metrics such as training progress, loss curve, and learning rate changes.
- **Checkpoint Management**: Automatically saves training checkpoints and supports resuming training from breakpoints.
- **Model Evaluation**: Comprehensive evaluation of multiple checkpoints, automatically recommending the optimal checkpoint.
- **Interactive Testing**: Select checkpoints for real-time inference testing to verify model response quality.
- **One-Click Packaging**: Export training artifacts to GGUF format for direct use in inference deployment.

---

## Interface Layout

The training module adopts a tab-based layout, containing five functional pages:

| Tab | Icon | Description |
|------|------|------|
| **Prepare** | Slider | Create training tasks and configure training parameters |
| **Monitor** | Line Chart | View training progress and real-time metrics |
| **Evaluate** | Pie Chart | Evaluate the quality of checkpoints |
| **Test** | Send | Interactively test checkpoint effects |
| **Package** | Save | Export model to GGUF format |

---

## Functional Modules

### Prepare Module

The Prepare module is used to create and configure training tasks:

| Function | Description |
|------|------|
| Training Type Selection | SFT (Supervised Fine-Tuning) or Pre-training |
| Data File Configuration | Training data and evaluation data file paths |
| Model Path Settings | Base model path and tokenizer path |
| Output Directory Specification | Storage location for checkpoints, logs, and other training artifacts |
| Parameter Template Selection | Quickly load preset parameter combinations |
| Parameter Fine-Tuning | Adjust various training hyperparameters item by item |

### Monitor Module

The Monitor module provides two working modes:

**Real-time Monitoring Mode**:
- Automatically enters when a training task is running.
- Displays current steps, total steps, progress percentage, and estimated remaining time in real-time.
- Real-time plotting of curves for Loss, Perplexity, Learning Rate, Gradient Norm, etc.
- Displays real-time training logs.

**History Review Mode**:
- Select completed training tasks to view historical data.
- Load and display complete training curves.
- Review training logs and checkpoint records.

### Evaluate Module

The Evaluate module is used to assess the model quality of multiple checkpoints:

| Function | Description |
|------|------|
| Checkpoint Selection | Support selecting 2 or more checkpoints for comparative evaluation |
| Evaluation Dataset | Use sampled data to test model generation capabilities |
| Comprehensive Scoring | Scoring based on multiple dimensions such as perplexity, semantic similarity, coherence, coverage, etc. |
| Result Output | Generate detailed evaluation reports and CSV result files to the output directory |
| Optimal Recommendation | Automatically identify the checkpoint with the highest comprehensive score |

Evaluation result files are stored in the task output directory.

### Test Module

The Test module provides interactive inference testing:

| Function | Description |
|------|------|
| Checkpoint Loading | Select any checkpoint for testing |
| Parameter Adjustment | Inference parameters such as Temperature, Top-P, Top-K, Repetition Penalty, etc. |
| Real-time Chat | Send messages to the model and view replies in real-time |
| System Prompt | Customize system prompts to test different role effects |

### Package Module

The Package module is used to export deployable models:

| Function | Description |
|------|------|
| Checkpoint Selection | Select the LoRA checkpoint to be packaged |
| Quantization Type | Support multiple quantization formats (Q2_K to Q8_0, F16, etc.) |
| Merge LoRA | Choose whether to merge LoRA weights into the base model |
| Output Format | Export to GGUF format, directly usable for inference |

---

## Training Task Mechanism

### Task Lifecycle

Training tasks transition through the following states:

```
pending → running → completed
                 ↘ stopped (stopped by user)
                 ↘ failed (abnormal exit)
```

| State | Description |
|------|------|
| `pending` | Task created, waiting to start |
| `running` | Training in progress |
| `stopping` | Stopping (transitional state) |
| `stopped` | Stopped by user |
| `completed` | Training completed normally |
| `failed` | Training exited abnormally |

### Task ID

A unique Task ID is generated when each training task is created, used for:
- Associating training logs
- Associating checkpoint records
- Associating evaluation, testing, and packaging operations

### Related Data Tables

The training module involves the following core data:

| Data Type | Description |
|----------|------|
| Training Task | Basic task information, status, configuration |
| Task Parameters | Specific training parameter values for the task |
| Training Log | Metrics such as loss, lr, progress for each step |
| Checkpoint Record | Save time, steps, and loss value for each checkpoint |

---

## Quota Consumption Rules

Quota consumption during training is related to checkpoint saving:

### Consumption Rules

- **Saving one checkpoint consumes 1 training quota**

### Influencing Factors

The number of saved checkpoints is affected by the following parameters:

| Interface Display Name | Description |
|-------------|------|
| Save Strategy | By steps (`steps`), by epoch (`epoch`), or no save (`no`) |
| Save Steps | Save every X steps when strategy is `steps` |
| Max Saved Checkpoints | Automatically delete old checkpoints after exceeding this number |
| Save Interval Minutes | Save by time interval (minutes) |

### Quota Estimation

**Example**: Total training steps 1000, save steps set to 100, then approximately 10 checkpoints will be saved, consuming 10 quotas.

### View Quota

Quota usage can be viewed in the application top status bar, and the system will issue a warning notification when the quota is insufficient.

---

## Training Parameter System

Training parameters are organized by function and support two template types: SFT and Pre-training.

> **Parameter Name Note**: The "Interface Display Name" column in the table below matches the parameter labels in the application interface for easy reference.

### Parameter Categories

| Category | Description | Key Parameters |
|------|------|----------|
| **data** | Data Configuration | Training data, evaluation data, model directory, tokenizer, etc. |
| **training** | Training Configuration | Batch size, learning rate, epochs, save strategy, etc. |
| **lora** | LoRA Configuration | Enable status, r, alpha, dropout, target modules |
| **optimization** | Optimization Configuration | Mixed precision (FP16/BF16/TF32), gradient checkpointing |
| **logging** | Log Configuration | Log level, logging steps |
| **general** | General Configuration | Random seed |

### Core Parameter Description

**Data Configuration**:

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Training data file path | - | Training data file in JSONL format (Required) |
| (opt.) Evaluation data file path | - | Evaluation data file in JSONL format |
| Training output directory | - | Storage location for checkpoints, logs, etc. (Required) |
| Training model path | - | **Directory path of the trainable model** (Required, see note below) |
| (opt.) Tokenizer path | - | Tokenizer directory path, leave blank to use model's own tokenizer |
| (opt.) Checkpoint path for resume | - | Continue training from specified checkpoint |
| Maximum samples | 0 | Limit number of training samples, 0 means unlimited |
| Evaluation sampling ratio | 0 | Ratio of evaluation set split from training set (0-1) |

> ⚠️ **About "Training model path"**:
> 
> This parameter needs to specify the directory where the **trainable model** is located, **not** the GGUF inference model in the Model Library.
> 
> - **Correct Format**: HuggingFace format model directory, containing `.safetensors` weight files and `config.json` configuration file.
> - **Incorrect Format**: `.gguf` file (only for inference, not for training).
> 
> Please ensure the model directory contains the following necessary files:
> - `config.json` - Model configuration
> - `*.safetensors` - Model weight files
> - `tokenizer.json` or `tokenizer_config.json` - Tokenizer configuration (optional, can also be specified separately)

**Training Configuration**:

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Batch size | 2 | Number of samples processed per step (1-128) |
| Gradient accumulation steps | 4 | Update weights after accumulating how many steps (1-256) |
| Max sequence length | 512 | Maximum length of input sequence (128-8192) |
| Learning rate | 2e-4 | Step size for model parameter updates (1e-6 to 1e-1) |
| Optimizer | adamw_torch | Optimization algorithm type |
| LR scheduler type | cosine | Learning rate change strategy |
| Training epochs | 2 | Number of times to traverse the complete dataset (1-100) |
| Weight decay | 0.01 | Regularization parameter to prevent overfitting |
| Warmup ratio | 0.1 | Proportion of learning rate warmup phase |
| Max gradient norm | 1.0 | Gradient clipping threshold |
| Save strategy | steps | Checkpoint save strategy: steps/epoch/no |
| Save steps | 100 | Save checkpoint every X steps |
| Evaluation strategy | no | Evaluation strategy during training: steps/epoch/no |
| Evaluation steps | 100 | Evaluate every X steps |
| Max saved checkpoints | 100 | Maximum number of checkpoints to retain |
| Early stopping patience | 0 | Step threshold to stop training when evaluation metrics do not improve |

**LoRA Configuration** (Enabled by default for SFT):

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Use LoRA fine-tuning | true | Whether to enable LoRA efficient fine-tuning |
| LoRA r parameter | 16 | LoRA rank, affects parameter count (1-512) |
| LoRA alpha parameter | 16 | LoRA scaling factor |
| LoRA dropout | 0.0 | Dropout ratio of LoRA layers (0-1) |
| LoRA target modules | ["q_proj", "v_proj"] | Model modules to apply LoRA |
| Merge LoRA weights after training | false | Whether to merge LoRA into base model after training |

**Optimization Configuration**:

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Use FP16 mixed precision | true | Use half-precision floating point to accelerate training |
| Use BF16 mixed precision (NVIDIA GPU) | false | Use BF16 format (Requires Ampere architecture or above) |
| Use TF32 (NVIDIA GPU) | false | Use TensorFloat-32 format |
| Gradient checkpointing | true | Trade time for space, reduce VRAM usage |

**Log Configuration**:

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Log level | info | Log detail level: debug/info/warning/error |
| Logging steps | 10 | Record logs every X steps |

**General Configuration**:

| Interface Display Name | Default | Description |
|-------------|--------|------|
| Random seed | 42 | Control randomness to ensure reproducibility (0-999999) |

### Difference between SFT and Pre-training

| Configuration Item | SFT | Pre-training |
|--------|-----|--------|
| LoRA | Enabled by default | Disabled by default (LoRA configuration area hidden) |
| Data Format | Dialogue format JSONL | Plain text data |
| Goal | Instruction following capability | Language modeling capability |

---

## Parameter Template Mechanism

### Template Types

The system supports two types of parameter templates:

| Type | Description |
|------|------|
| **Default Template** | System preset basic parameter configuration, distinguished by training type (SFT/Pre-training) |
| **User Template** | Parameter configuration customized and saved by the user |

### Temporary Parameters

When creating a training task:

1. First load the selected parameter template.
2. Users can adjust various parameters on this basis.
3. Adjusted parameters are saved as **Task Parameters**, independent of the template.
4. Subsequent modifications to the template do not affect created tasks.

### Parameter Folding

Parameters on the interface are displayed in layers according to importance:

| Fold Group | Description |
|--------|------|
| No Fold | Core parameters, expanded by default |
| `optional` | Optional parameters, folded by default |
| `advanced` | Advanced parameters, folded by default |
| `optimization` | Optimization related parameters, folded by default |
| `save` | Save strategy parameters, folded by default |
| `eval` | Evaluation strategy parameters, folded by default |

---

## Next Steps

- [Prepare Module Details](./prepare.md) - Guide to training task creation and parameter configuration
- [Monitor Module Details](./monitor.md) - Real-time monitoring and history review functions
- [Evaluate Module Details](./evaluate.md) - Checkpoint evaluation and comparative analysis
- [Test Module Details](./test.md) - Interactive testing function
- [Package Module Details](./package.md) - GGUF export and quantization options
