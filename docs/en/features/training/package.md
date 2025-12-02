# Training Package Module Details

> v1.0.0
> 2025-12

> 📌 **Module Positioning**: Convert training artifacts into deployable GGUF format, supporting weight merging, format conversion, and quantization compression.

## Table of Contents
- [Overview](#overview)
- [Configuration Templates (Recommended)](#configuration-templates-recommended)
  - [Template Overview](#template-overview)
  - [Template Details](#template-details)
- [Basic Configuration Parameters](#basic-configuration-parameters)
- [Packaging Options Details](#packaging-options-details)
- [Option Dependencies](#option-dependencies)
- [Packaging Flow](#packaging-flow)
- [Packaging Log](#packaging-log)
- [FAQ](#faq)
- [Usage Suggestions](#usage-suggestions)
- [Next Steps](#next-steps)

---

## Overview

The Package module is responsible for converting trained models (including base model and LoRA weights) into GGUF format suitable for inference deployment. Due to many parameters and mutual exclusion/dependency relationships, **it is strongly recommended to use preset templates** for operation.

### Core Functions

| Function | Description |
|------|------|
| LoRA Weight Merge | Merge LoRA adapter weights into base model |
| GGUF Format Conversion | Convert HuggingFace format model to GGUF |
| Model Quantization | Compress model size, support multiple quantization precisions |
| LoRA Independent Export | Export LoRA weights independently as GGUF format |

### Output Artifacts

| File Type | Naming Format | Description |
|----------|----------|------|
| Unquantized GGUF | `{model_name}.gguf` | GGUF file with original precision |
| Quantized GGUF | `{model_name}-{Q_TYPE}.gguf` | Quantized GGUF file |
| LoRA GGUF | `{checkpoint_name}-lora.gguf` | GGUF file of LoRA adapter |

---

## Configuration Templates (Recommended)

> 💡 **Suggestion**: Due to complex dependencies between packaging options, it is strongly recommended to use preset templates.

### Template Overview

| Template | Description | Applicable Scenario |
|------|------|----------|
| **Merge and Quantize (Default)** | Merge LoRA + Export GGUF + Quantize Q4_K_M | Standard packaging flow after SFT training |
| **Merge without Quantize** | Merge LoRA + Export GGUF (No Quantization) | Retain full precision, self-quantize later |
| **Quantize Base Model Only** | Export GGUF + Quantize (No LoRA Merge) | Pre-trained model or full fine-tuning |
| **Export LoRA Only** | Export LoRA adapter as GGUF only | Scenarios requiring independent use of LoRA |
| **Custom** | Fully autonomous control of all options | Advanced users who understand parameter relationships |

---

### Template Details

#### Template 1: Merge Base Model and LoRA Weights, and Quantize (Default)

**Option Configuration**:
| Option | Value |
|------|-----|
| Merge LoRA weights to base model | ✅ On |
| Export base model as GGUF | ✅ On |
| Quantize model | ✅ On |
| Quantization type | Q4_K_M |
| Export LoRA as GGUF | ❌ Off |

**Execution Flow**:
```
Base Model + LoRA Weights → Merge → Convert GGUF → Quantize → Output
```

**Output Files**:
- `merged_model.gguf` (Unquantized version)
- `merged_model-Q4_K_M.gguf` (Quantized version)

**Applicable Scenarios**:
- Standard packaging after SFT fine-tuning.
- Need to deploy to local inference engine.

---

#### Template 2: Merge Base Model and LoRA Weights, No Quantization

**Option Configuration**:
| Option | Value |
|------|-----|
| Merge LoRA weights to base model | ✅ On |
| Export base model as GGUF | ✅ On |
| Quantize model | ❌ Off |
| Export LoRA as GGUF | ❌ Off |

**Execution Flow**:
```
Base Model + LoRA Weights → Merge → Convert GGUF → Output
```

**Output Files**:
- `merged_model.gguf` (Full precision)

**Applicable Scenarios**:
- Need to retain full precision.
- Plan to use other tools for quantization.
- Have sufficient VRAM/RAM to run full model.

---

#### Template 3: Quantize Base Model Only

**Option Configuration**:
| Option | Value |
|------|-----|
| Merge LoRA weights to base model | ❌ Off |
| Export base model as GGUF | ✅ On |
| Quantize model | ✅ On |
| Quantization type | Q4_K_M |
| Export LoRA as GGUF | ❌ Off |

**Execution Flow**:
```
Base Model → Convert GGUF → Quantize → Output
```

**Output Files**:
- `{model_name}.gguf` (Unquantized version)
- `{model_name}-Q4_K_M.gguf` (Quantized version)

**Applicable Scenarios**:
- Model packaging for Pre-training tasks.
- Model packaging for full fine-tuning (non-LoRA).
- Only need to quantize existing HuggingFace model.

---

#### Template 4: Export LoRA Only

**Option Configuration**:
| Option | Value |
|------|-----|
| Merge LoRA weights to base model | ❌ Off |
| Export base model as GGUF | ❌ Off |
| Quantize model | ❌ Off |
| Export LoRA as GGUF | ✅ On |

**Execution Flow**:
```
LoRA Weights → Convert LoRA GGUF → Output
```

**Output Files**:
- `{checkpoint_name}-lora.gguf`

**Applicable Scenarios**:
- Need to distribute LoRA weights independently.
- Base model already has GGUF version, only need to supplement LoRA.
- Test different LoRA combinations.

---

## Basic Configuration Parameters

### Training task selection

| Attribute | Value |
|------|-----|
| Type | Dropdown Select |
| Required | ✅ Yes |

**Description**: Select the training task to package.

**Optional Tasks**:
- Tasks with status "Completed"
- Tasks with status "Stopped"
- Tasks with status "Failed" (Available in some scenarios)

**Auto-fill after selection**:
- Base model path
- Output directory
- LoRA checkpoint list (if LoRA task)

---

### Base model path

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ✅ Yes |

**Description**: HuggingFace format base model directory.

**Auto-fill**: Automatically filled as the task's training model path after selecting task.

**Requirements**:
- Directory must contain `.safetensors` format model weights.
- Must contain `config.json` and other configuration files.
- Suggest containing `tokenizer.model` (SentencePiece tokenizer file).

> ⚠️ **Note**: If `tokenizer.model` is missing in the directory, the system will try to backtrack from the base model path, but may fail.

---

### LoRA checkpoint selection

| Attribute | Value |
|------|-----|
| Type | Dropdown Select |
| Required | LoRA Task Only |
| Condition | Display only when task type is SFT and uses LoRA |

**Description**: Select the LoRA checkpoint to package.

**Checkpoint Source**: `checkpoint-*` subdirectories under task output directory.

**Selection Suggestion**:
- Refer to Evaluation Module scores to select best checkpoint.
- Usually select checkpoint at the end of training (larger steps).
- If early stopping occurred, select the last checkpoint before early stopping.

---

### Output directory

| Attribute | Value |
|------|-----|
| Type | Directory Path |
| Required | ✅ Yes |

**Description**: Output directory for packaging artifacts.

**Auto-fill**: Defaults to task output directory.

**Suggestion**:
- Use a dedicated directory to store packaging results.
- Ensure sufficient disk space (files are still large after quantization).

---

## Packaging Options Details

### Merge LoRA weights to base model

| Attribute | Value |
|------|-----|
| Type | Switch |
| Default | On |
| Custom Mode | ✅ Modifiable |
| Template Mode | ❌ Locked |

**Description**: Merge LoRA adapter weights into base model.

**When On**:
- Generate complete model weights.
- No need to load LoRA separately during inference.
- File size equals complete model.

**When Off**:
- LoRA weights remain independent.
- Need to be used with base model.
- Support dynamic switching of different LoRA.

---

### Export base model as GGUF

| Attribute | Value |
|------|-----|
| Type | Switch |
| Default | On |
| Custom Mode | ✅ Modifiable |
| Template Mode | ❌ Locked |

**Description**: Convert HuggingFace format model to GGUF format.

**GGUF Format Advantages**:
- Native support by llama.cpp.
- Single file contains all weights and configurations.
- Support incremental loading, reducing memory requirements.

**Conversion Process**:
1. Read HuggingFace model weights.
2. Convert tensor format.
3. Package tokenizer configuration.
4. Write GGUF single file.

---

### Quantize model

| Attribute | Value |
|------|-----|
| Type | Switch |
| Default | On |
| Custom Mode | ✅ Modifiable |
| Template Mode | ❌ Locked |
| Dependency | Need to enable "Export GGUF" first |

**Description**: Perform quantization compression on GGUF model.

**Quantization Effect**:
- Significantly reduce model size.
- Reduce memory requirements during inference.
- Moderate loss of precision (usually acceptable).

---

### Quantization types

| Attribute | Value |
|------|-----|
| Type | Multi-select Dropdown |
| Default | Q4_K_M |
| Condition | Display only when "Quantize model" is enabled |

**Description**: Select one or more quantization precisions.

#### Quantization Types Details

**K-Quant Series (Recommended)**:

| Type | Size | Precision Loss | Recommended Scenario |
|------|------|----------|----------|
| **Q4_K_M** ⭐ | ~4.6G | Low | **Default Recommended**, balance size and quality |
| **Q5_K_M** ⭐ | ~5.3G | Very Low | Pursue higher quality |
| Q4_K_S | ~4.4G | Low | Size priority |
| Q5_K_S | ~5.2G | Very Low | Quality priority |
| Q3_K_M | ~3.7G | Medium | Extreme compression |
| Q3_K_L | ~4.0G | Medium Low | Compromise between compression and quality |
| Q3_K_S | ~3.4G | Medium | Smallest 3-bit |

**Standard Quantization Series**:

| Type | Size | Description |
|------|------|------|
| Q8_0 | ~8.0G | 8-bit quantization, quality close to original |
| Q5_0 | ~5.2G | 5-bit standard quantization |
| Q5_1 | ~5.7G | 5-bit enhanced quantization |
| Q4_0 | ~4.3G | 4-bit standard quantization |
| Q4_1 | ~4.8G | 4-bit enhanced quantization |
| Q2_K | ~3.0G | 2-bit extreme compression |

**IQ Series (Intelligent Quantization)**:

| Type | bpw | Description |
|------|-----|------|
| IQ4_XS | 4.25 | Intelligent 4-bit |
| IQ4_NL | 4.50 | Non-linear 4-bit |
| IQ3_M | 3.66 | Intelligent 3-bit Medium |
| IQ3_S | 3.44 | Intelligent 3-bit Small |
| IQ2_M | 2.70 | Intelligent 2-bit Medium |
| IQ2_S | 2.50 | Intelligent 2-bit Small |
| IQ1_M | 1.75 | Intelligent 1-bit Medium |
| IQ1_S | 1.56 | Intelligent 1-bit Small |

**Full Precision Formats**:

| Type | Size | Description |
|------|------|------|
| F32 | ~26G | 32-bit Float (Original Precision) |
| F16 | ~14G | 16-bit Float |
| BF16 | ~14G | BFloat16 Format |

**Selection Suggestion**:

| Scenario | Recommended Type |
|------|----------|
| General Deployment | Q4_K_M (Default) |
| Pursue Quality | Q5_K_M or Q8_0 |
| Memory Limited | Q3_K_M or IQ3_M |
| Extreme Compression | Q2_K or IQ2_S |
| Multi-version Distribution | Select multiple types to generate simultaneously |

---

### Export LoRA as GGUF

| Attribute | Value |
|------|-----|
| Type | Switch |
| Default | Off |
| Custom Mode | ✅ Modifiable |
| Template Mode | ❌ Locked |

**Description**: Export LoRA adapter independently as GGUF format.

**Usage Scenarios**:
- Base model already has GGUF version.
- Need to distribute lightweight LoRA files.
- Test effects of different LoRA.

**Output Files**:
- `{checkpoint_name}-lora.gguf` (Very small size, usually MB level)

---

## Option Dependencies

### Mutual Exclusion

| Option A | Option B | Relationship Description |
|--------|--------|----------|
| Merge LoRA | Export LoRA Only | Choose one, no independent LoRA after merge |

### Dependency Relationship

| Prerequisite | Dependent Option | Description |
|----------|----------|------|
| Export GGUF = On | Quantize Model | Quantization requires GGUF file first |
| Quantize Model = On | Quantization Type Selection | Type selection displays only after quantization enabled |
| LoRA Task | LoRA Checkpoint Selection | Not displayed for non-LoRA tasks |

### Flow Dependency

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Merge LoRA │ ──▶ │ Export GGUF │ ──▶ │  Quantize   │
└─────────────┘     └─────────────┘     └─────────────┘
      │                                        │
      │                                        ▼
      │                               ┌─────────────────┐
      │                               │ Quant Type(Multi)│
      │                               └─────────────────┘
      ▼
┌─────────────────┐
│ Export LoRA GGUF│ (Mutually exclusive with Merge)
└─────────────────┘
```

---

## Packaging Flow

### Standard Flow (Using Template)

1. **Select Task**
   - Select completed training task from dropdown list.
   - System automatically fills path information.

2. **Select Template**
   - Select appropriate preset template according to needs.
   - Confirm configuration summary displayed on the right.

3. **Select Checkpoint** (LoRA Task)
   - Select version to package from checkpoint list.
   - Refer to evaluation score to select best checkpoint.

4. **Confirm Output Directory**
   - Check if output directory is correct.
   - Ensure sufficient disk space.

5. **Start Packaging**
   - Click "Start Packaging" button.
   - Wait for packaging to complete.

### Custom Flow

1. **Select Task and Checkpoint**

2. **Select "Custom" Template**
   - All options become editable at this time.

3. **Configure Options**
   - Set item by item according to needs.
   - Pay attention to dependencies between options.

4. **Start Packaging**

---

## Packaging Log

Real-time logs are displayed during packaging:

| Log Type | Description |
|----------|------|
| Merge LoRA | LoRA weight merge process |
| GGUF conversion | Format conversion process |
| Quantization | Quantization process |
| Command success | Step completed |

---

## FAQ

### Packaging Failed: tokenizer.model not found

**Reason**: Tokenizer file is missing in directory after LoRA merge.

**Solution**:
1. Ensure base model directory contains `tokenizer.model`.
2. Manually copy `tokenizer.model` to merged directory.
3. Re-execute packaging.

### Quantization Failed

**Possible Reasons**:
- GGUF file not generated correctly.
- Insufficient disk space.
- Quantization tool missing.

**Solution**:
1. Check if previous GGUF conversion was successful.
2. Ensure sufficient disk space (at least 2 times model size).
3. Check if `llama-quantize` tool exists.

### Output File Too Large

**Solution**:
- Select more aggressive quantization type (e.g., Q3_K_M, Q2_K).
- Delete unwanted intermediate files (unquantized .gguf).

### LoRA Checkpoint List Empty

**Possible Reasons**:
- Task output directory does not exist.
- Training did not produce checkpoints.
- Non-LoRA task.

**Solution**:
1. Confirm task actually used LoRA training.
2. Check if `checkpoint-*` directories exist under task output directory.
3. Ensure checkpoint saving was set during training.

---

## Usage Suggestions

### Quick Start

For most users:
1. Select training task.
2. Use default template (Merge and Quantize).
3. Select checkpoint.
4. Start packaging.

### Advanced Usage

- **Multi-Quantization Versions**: Select multiple quantization types to generate multiple versions at once.
- **Retain Original Precision**: Use "Merge without Quantize" template, quantize with other tools later.
- **Independent LoRA**: Use "Export LoRA Only" template, use with existing base model.

### Deployment Suggestions

| Deployment Platform | Recommended Quantization Type |
|----------|--------------|
| High-end GPU (24G+) | Q8_0 or F16 |
| Mid-range GPU (12G) | Q5_K_M |
| Low-end GPU (8G) | Q4_K_M |
| CPU Inference | Q4_K_M or Q3_K_M |
| Mobile | IQ3_M or IQ2_S |

---

## Next Steps

- [Overview](./overview.md) - Training function overview
- [Prepare Module Details](./prepare.md) - Task creation and parameter configuration
- [Monitor Module Details](./monitor.md) - Training monitoring
- [Evaluate Module Details](./evaluate.md) - Checkpoint evaluation
- [Test Module Details](./test.md) - Interactive testing
