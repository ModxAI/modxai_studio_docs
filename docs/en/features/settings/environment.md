# Environment Installation Guide

> v1.0.0
> 2025-12

This document provides a detailed introduction to the **Environment Configuration** feature of ModxAI Studio, helping you understand the principles, operational procedures, and troubleshooting methods for environment installation.

## Table of Contents

- [Feature Overview](#feature-overview)
- [Storage Location and Space Requirements](#storage-location-and-space-requirements)
- [GPU Detection Standards](#gpu-detection-standards)
- [NVIDIA Driver and CUDA Configuration](#nvidia-driver-and-cuda-configuration)
- [Environment Type Selection](#environment-type-selection)
- [Installation Content Details](#installation-content-details)
- [Operation Instructions](#operation-instructions)
- [Installation Process Details](#installation-process-details)
- [Troubleshooting](#troubleshooting)

---

## Feature Overview

Environment configuration is a core highlight feature of ModxAI Studio, adopting the following technical strategies:

| Feature | Description |
|---------|-------------|
| **One-Click Installation** | Automatically configure portable Python environment and all dependencies |
| **Sandbox Isolation** | Completely independent from system Python, no impact on existing development environments |
| **Differential Loading** | Install on demand, unused feature modules will not install corresponding dependencies |
| **Dynamic Loading** | Load libraries at runtime as needed, fast startup, low memory usage |
| **Resume Support** | Can continue after installation interruption, downloaded files will be retained |
| **Immediate Use** | No need to restart the application after installation, features immediately available |

---

## Storage Location and Space Requirements

### Installation Location

Environment dependency packages are installed in a persistent path under the **system user directory**:

| Operating System | Path Example |
|------------------|--------------|
| Windows | `C:\Users\<username>\AppData\Local\ModxAI\packages` |

> ⚠️ **Note**: The installation location is on the system drive, please ensure sufficient available space on the system drive.

### Space Requirements

| Environment Type | Minimum Available Space | Recommended Available Space |
|------------------|-------------------------|----------------------------|
| CPU Environment | **10 GB** | 15 GB |
| GPU Environment | **10 GB** | 20 GB |

> 💡 **Note**:
> - GPU environment needs to download CUDA version of PyTorch, which is larger in size
> - Additional space required if installing optional acceleration libraries (e.g., bitsandbytes)
> - Temporary files during installation will be automatically cleaned up after completion

---

## GPU Detection Standards

The system automatically detects graphics card types, with different support for various GPUs:

### NVIDIA Graphics Cards

| Detection Item | Detection Method | Pass Criteria |
|----------------|------------------|---------------|
| GPU Exists | Call `nvidia-smi` | Command executes successfully and returns GPU information |
| Driver Version | Parse `nvidia-smi` output | Driver version number is not empty |
| CUDA Version | Call `nvcc --version` | Return CUDA version ≥ 11.8 |
| VRAM Size | Parse `nvidia-smi` output | For display only, does not affect installation |

### AMD Graphics Cards

| Detection Item | Detection Method | Description |
|----------------|------------------|-------------|
| GPU Exists | Read Windows registry or WMI | Detect Radeon series discrete GPUs |
| VRAM Size | Read AdapterRAM | For display only |

> ⚠️ **AMD GPU Limitations**:
> - On Windows platform, AMD GPU currently has no official ROCm support
> - When selecting GPU environment, will **automatically fall back to CPU version of PyTorch**
> - AMD GPU users are recommended to directly select CPU environment, but this does not affect inference acceleration

### No Discrete GPU

If the system does not detect a discrete GPU (only integrated GPU or no GPU):

- GPU option will be disabled
- Only CPU environment can be installed
- All features work normally, but inference and training speed will be slower

---

## NVIDIA Driver and CUDA Configuration

> ⚠️ **Important Notice**: The environment installation feature **does not automatically install** NVIDIA drivers and CUDA Toolkit, users need to configure them manually.

### Driver Requirements

| Component | Minimum Version | Recommended Version | Download Link |
|-----------|-----------------|---------------------|---------------|
| NVIDIA Driver | 450.80+ | Latest stable | [NVIDIA Driver Download](https://www.nvidia.com/Download/index.aspx) |
| CUDA Toolkit | 11.8 | 12.0-12.9 | [CUDA Download](https://developer.nvidia.com/cuda-downloads) |
| cuDNN (Optional) | 8.6+ | 8.9+ | [cuDNN Download](https://developer.nvidia.com/cudnn) (requires account registration) |

### CUDA Version Mapping

The system automatically selects the appropriate PyTorch version based on the detected CUDA version:

| Local CUDA Version | Installed PyTorch Version |
|--------------------|---------------------------|
| CUDA 11.7 and below | Fall back to CUDA 11.8 version |
| CUDA 11.8 | PyTorch + CUDA 11.8 |
| CUDA 12.1 | PyTorch + CUDA 12.1 |
| CUDA 12.4 | PyTorch + CUDA 12.4 |
| CUDA 12.6 | PyTorch + CUDA 12.6 |
| CUDA 12.8+ | PyTorch + CUDA 12.8 |

### cuDNN Acceleration (Optional)

cuDNN is NVIDIA's deep learning acceleration library that can significantly improve training and inference speed:

- **Installation Recommendation**: Strongly recommended if performing model training and audio processing
- **Version Selection**: Choose cuDNN matching your CUDA version (e.g., CUDA 12.x with cuDNN 9.x)
- **Installation Method**: After downloading and extracting, copy contents of `bin`, `include`, and `lib` directories to CUDA installation directory

### Verify CUDA Installation

Execute the following commands in command line to verify:

```bash
# Check NVIDIA driver
nvidia-smi

# Check CUDA version
nvcc --version
```

If the command fails or reports "command not found", it means the driver or CUDA is not correctly installed or not added to system PATH.

---

## Environment Type Selection

### CPU Environment

| Use Case | Description |
|----------|-------------|
| No Discrete GPU | Devices with integrated GPU or no GPU |
| AMD GPU | Windows platform AMD GPU does not support training GPU acceleration yet, but supports inference |
| Light Usage | Only small-scale data processing or inference testing |
| Development/Debugging | Feature verification without performance requirements |

**Characteristics**:
- Smaller download size (approximately 2-3 GB)
- No CUDA environment dependency
- All features available but slower speed

### GPU Environment

| Use Case | Description |
|----------|-------------|
| NVIDIA GPU | Has NVIDIA discrete GPU with CUDA installed |
| Model Training | Training tasks requiring GPU acceleration |
| Large-Scale Inference | Batch processing or real-time inference scenarios |
| Performance Priority | Pursuit of optimal processing speed |

**Characteristics**:
- Larger download size (approximately 4-6 GB, including CUDA version PyTorch)
- Requires correctly configured NVIDIA driver and CUDA
- Training speed improvement of 10-100x (depending on GPU performance)

### Environment Mutual Exclusion

> ⚠️ **Important**: CPU environment and GPU environment **cannot be installed simultaneously**, they are mutually exclusive.

- Before switching environment type, need to **clean** the currently installed environment first
- If another environment is detected, the install button will be disabled
- Interface will prompt "Environment conflict detected", guiding users to clean first

---

## Installation Content Details

### Portable Python

| Component | Version | Description |
|-----------|---------|-------------|
| Python | 3.11.9 | Windows embeddable version, completely independent |
| pip | Latest | Package management tool |

Portable Python will be downloaded and extracted first, with all subsequent dependencies installed to the isolated `packages` directory.

### Core Dependencies

Install corresponding versions of core libraries based on environment type:

| Library Name | CPU Version | GPU Version | Purpose |
|--------------|-------------|-------------|---------|
| PyTorch | torch (CPU) | torch + CUDA | Deep learning framework |
| torchvision | CPU version | CUDA version | Image processing |
| torchaudio | CPU version | CUDA version | Audio processing |

### Base Feature Libraries

The following base libraries will be installed regardless of CPU or GPU environment selection:

| Library Name | Version | Purpose |
|--------------|---------|---------|
| transformers | 4.52+ | Hugging Face model library |
| sentence-transformers | 4.0+ | Vector model inference |
| trl | 0.15+ | Reinforcement learning training |
| peft | 0.15+ | Efficient fine-tuning (LoRA, etc.) |
| sentencepiece | 0.2+ | Tokenizer |
| faster-whisper | 1.2+ | Speech recognition |
| pandas | 2.3+ | Data processing |
| matplotlib | 3.10+ | Chart plotting |
| jieba | 0.42+ | Chinese word segmentation |
| spacy | 3.8+ | NLP processing |
| markitdown | 0.1+ | Markdown parsing |
| mistral_common | 1.8+ | Mistral model support |

### Optional Acceleration Libraries (GPU Environment)

GPU environment installation will **asynchronously attempt** to install the following optional libraries, failure does not affect main process:

| Library Name | Purpose | Description |
|--------------|---------|-------------|
| bitsandbytes | 4/8-bit quantization | Reduce VRAM usage, accelerate training |

### TTS Optional Dependencies

TTS (Text-to-Speech) feature requires the following optional dependencies, **installed asynchronously** after main environment installation:

| Library Name | Purpose | Description |
|--------------|---------|-------------|
| misaki[en,zh] | G2P conversion | Grapheme-to-Phoneme conversion, supports Chinese and English |

> ⚠️ **TTS Dependency Installation Notes**:
> - misaki library is forcibly installed from **PyPI official source**, as domestic mirror sources have incomplete extras dependency resolution
> - Users in mainland China may experience slow downloads or failures, please maintain stable network connection
> - Installation failure does not affect main environment, can retry via **Reinstall** or **Repair Environment**
> - After successful installation, refresh environment status to confirm if TTS capability is available

### Optional Model Resources

After installation completion, the following model resources will be downloaded asynchronously:

| Resource | Size | Purpose |
|----------|------|---------|
| en_core_web_sm | ~14 MB | spaCy English model (NLP processing, TTS English G2P) |
| zh_core_web_sm | ~75 MB | spaCy Chinese model (Chinese segmentation, TTS Chinese G2P) |

> 💡 **Note**:
> - Model resource download failure does not affect environment installation status, you can try downloading again when using related features later. 
> - **TTS Feature Dependency**: TTS speech synthesis feature requires spaCy models for G2P conversion
> - **Asynchronous Download Delay**: These models are **downloaded asynchronously** after main environment installation completes, download time depends on network conditions and may be delayed by tens of seconds to several minutes
> - **Capability Status**: After main environment installation completes, please **refresh environment status** to confirm if various capabilities (including TTS) are available
> - If TTS capability shows as unavailable, can try **restart application** or **repair installation** to download related dependencies again

---

## Operation Instructions

### Refresh

Clicking the **Refresh** button executes the following operations:

1. Re-detect system hardware information (CPU, memory, GPU)
2. Re-detect NVIDIA driver and CUDA version
3. Re-detect installed dependency package status
4. Run verification script to confirm environment availability

> ⚠️ **Note**: First refresh or refresh after environment changes may be slow (10-30 seconds) because verification scripts need to be executed to check versions and CUDA support status of various libraries.

### Install

Click the **Install** button to start the installation process:

1. Confirm installation (confirmation dialog displays selected mirror source)
2. Display progress bar and real-time installation logs
3. **Automatically refresh** environment status after installation completion
4. **No need to restart application** after successful installation, features immediately available

> ⚠️ **Note**: Try to maintain stable network connection during installation to avoid interruption. Unless necessary, do not manually stop installation.

### Reinstall

If environment is already installed, the install button will change to **Reinstall**:

- Reinstall will overwrite existing environment
- Suitable for damaged environment or need to update dependency versions

### Clean

Clicking the **Clean** button will delete the installed environment:

1. Confirm cleaning (confirmation dialog pops up)
2. Delete all dependencies in `packages` directory

> ⚠️ **Clean Failure Handling**:
> 
> If cleaning prompts "File in use" or "Clean failed", this is usually because some libraries (like PyTorch) have been loaded by the current process.
> 
> **Solution**:
> - Click the "Restart" button to restart the application
> - Execute clean operation again after restart
> - Can only switch to another environment type after cleaning is complete

### Stop Installation

During installation, you can click the **Stop Installation** button to interrupt:

- Downloaded packages will be retained and can be reused in next installation
- Environment status will be marked as "failed" after stopping
- Can restart installation at any time

---

## Installation Process Details

The complete installation process includes the following stages:

Start → Prepare Portable Python → Pre-clean Check → Install Training Environment 
    → Install Acceleration Libraries (Optional) → Install Base Dependencies → Download Models (Optional) 
    → Verify Environment → Complete

### Stage Descriptions

| Stage | Description | Estimated Time |
|-------|-------------|----------------|
| prepare_portable_python | Download/extract portable Python | 1-5 minutes |
| preclean | Check and clean incompatible old environments | < 1 minute |
| training_requirements | Install PyTorch and other training core libraries | 5-20 minutes |
| accelerate_optional | Asynchronously install optional acceleration libraries | Background process |
| base_requirements | Install transformers and other base libraries | 3-10 minutes |
| download_models | Asynchronously download spaCy models | Background process |
| verify | Verify environment integrity | 1-2 minutes |
| finalize | Refresh status and complete | < 1 minute |

> 💡 **Time Note**: Actual installation time depends on network speed and selected mirror source. Users in mainland China are recommended to select (Alibaba Cloud/Tencent Cloud) mirror sources which are usually faster.

---

## Troubleshooting

### Common Issues

#### 1. Installation Failed: Network Timeout

**Symptom**: Download failure or timeout prompt during installation

**Solution**:
- Switch mirror source
- Check network connection, ensure external network access
- Retry after disabling VPN or proxy software
- Click "Install" to retry, downloaded files will be reused

#### 2. Installation Failed: Insufficient Disk Space

**Symptom**: Insufficient disk space prompt

**Solution**:
- Clean temporary files on system drive (C drive)
- Ensure at least 10 GB available space
- Uninstall unnecessary software to free up space

#### 3. GPU Environment Detected as CPU After Installation

**Symptom**: After installing GPU environment, torch.cuda.is_available() returns False

**Possible Causes**:
- CUDA not correctly installed
- CUDA version does not match PyTorch version
- NVIDIA driver version too low

**Solution**:
1. Execute `nvidia-smi` in command line to confirm driver is normal
2. Execute `nvcc --version` to confirm CUDA is installed
3. If CUDA version is too low, upgrade to 11.8 or higher
4. Clean current environment and reinstall

#### 4. Clean Failed: File in Use

**Symptom**: "Some files may be occupied by the system" prompt during cleaning

**Solution**:
- Click "Restart" button to restart application
- Execute clean operation immediately after restart
- If still fails, manually restart computer and try again

#### 5. Slow Refresh

**Symptom**: Long wait after clicking refresh

**Cause**: First refresh needs to execute verification script to check versions and CUDA support status of various libraries

**Note**: This is normal behavior, subsequent refreshes will use cache and speed will be significantly faster

#### 6. Environment Conflict Prompt

**Symptom**: "Core packages of another environment detected" prompt

**Solution**:
1. First click "Clean" to delete current environment
2. If cleaning fails, restart application and clean again
3. Install target environment after successful cleaning

### Installation Logs

Installation logs are displayed in real-time in the progress card:

- Expand "Installation Log" to view detailed information
- Log content can help locate specific causes of installation failure
- Please provide complete installation logs if technical support is needed

---

## Related Documentation

- [Global Settings Overview](./overview.md) - Return to settings feature overview
- [License Activation Guide](./license.md) - License type description and activation process

