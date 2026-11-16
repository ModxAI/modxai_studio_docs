# Global Settings

> v1.1.11
> 2026-11

Use Settings to install environments, adjust app options, change the theme, and check for updates.

## Table of Contents

- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Environment Configuration](#environment-configuration)
- [General Settings](#general-settings)
- [Global Parameters](#global-parameters)
- [Theme Settings](#theme-settings)
- [Version Updates](#version-updates)
- [Help & Feedback](#help--feedback)
- [MCP External Tools](#mcp-external-tools)
- [Related Documentation](#related-documentation)

---

## Feature Overview

Use Settings to install environments, change the interface language and theme, and adjust how the app starts and closes.

---

## Interface Layout

Click **Settings** at the bottom of the main menu, then choose a category:

| Category | Contents |
|----------|----------|
| **General** | Language, window behavior, message send shortcut, and global parameters |
| **System** | Environment installation and CUDA inference libraries |
| **Agent** | Message mode, global instruction file, search ignore file, and user preferences |
| **Credentials** | Online model connections and credentials |
| **MCP** | External tool connections and authorization |
| **Appearance** | Themes and appearance |
| **Help** | App information, updates, documentation, and feedback |

---

## Environment Configuration

Open **Settings → System**. Install environment dependencies and CUDA inference libraries separately as needed for your features.

Environment Configuration is one of the **core highlights** of ModxAI Studio, designed to lower the barrier for environment setup.

### Design Philosophy

- **Zero-Barrier Installation**: Users don't need to manually configure Python, PyTorch, or other complex environments
- **Differential Loading**: Install dependencies on demand; training packages only installed when training features are needed
- **Sandbox Isolation**: Completely independent environment, not dependent on system Python, avoiding conflicts
- **Resume Support**: Installation can continue after interruption; downloaded packages are retained

### CUDA Inference Libraries

When an NVIDIA GPU is detected, **Settings → System** shows **CUDA Inference Library Installation**:

1. Select **Text Generation** or **Image Generation** for your task.
2. Click **Install** and wait for completion. Click **Stop** if you need to interrupt it.
3. Reload the relevant model after installation to use the new library.

### System Information

The Environment Configuration section automatically detects and displays current system information:

| Item | Description |
|------|-------------|
| System | Operating system type. Path examples in these docs are written for Windows |
| CPU | CPU core count |
| Memory | Total system memory (GB) |
| GPU | Graphics card type (NVIDIA/AMD) |
| Driver | NVIDIA driver version (displayed for NVIDIA GPU only) |

### Basic System Requirements

The system checks whether the following requirements are met:

| Check Item | Description |
|------------|-------------|
| Platform | Operating system compatibility |
| Python | Built-in portable Python environment |
| Memory | 16GB or more recommended |
| Disk | Sufficient installation space required |
| GPU | Optional; NVIDIA GPU enables GPU acceleration |
| NVIDIA Driver | Required for GPU environment |
| CUDA Version | Required for GPU environment |

### Installation Controls

| Operation | Description |
|-----------|-------------|
| **Environment Type Selection** | Choose CPU or GPU environment (GPU requires NVIDIA graphics card) |
| **Mirror Source Selection** | Official (pypi.org) / Alibaba Cloud / Tencent Cloud; select based on your region's network conditions |
| **Refresh** | Re-detect environment status |
| **Clean** | Remove installed environment dependencies |
| **Install / Reinstall** | Start installation or reinstall environment |

### Installation Process

1. Select environment type (CPU or GPU)
2. Select mirror source (users in Chinese mainland are recommended to choose Alibaba Cloud or Tencent Cloud)
3. Click the "Install" button
4. Wait for installation to complete (progress bar and logs displayed)
5. After successful installation, related feature modules become available without restarting the application

### Feature Availability

After installation, feature availability icons are displayed in the upper right corner:

| Feature | Description |
|---------|-------------|
| Model Management | Basic functions like model loading and unloading |
| Data Processing | Dataset creation and processing functions |
| Training | Model fine-tuning and training functions |

> For detailed environment installation guide, please refer to [Environment Installation Guide](./environment.md)

---

## General Settings

Open **Settings → General** to configure app behavior.

### Editor

Under **Message Send Shortcut**, choose `Enter` or `Ctrl + Enter`. This applies to the agent, AI Chat, image generation, and speech synthesis input boxes. `Shift + Enter` always inserts a new line.

Choose the agent’s default **Queue / Steer** mode separately under **Settings → Agent → Messages**. See [Send Modes](../agent/overview.md#send-modes).

### Select Language

| Option | Description |
|--------|-------------|
| English | English interface |
| 中文 (简体) | Simplified Chinese interface |

After switching languages, the entire application interface updates immediately to the corresponding language.

### Close Behavior

Set the behavior when clicking the window close button:

| Option | Description |
|--------|-------------|
| **Minimize to Tray** | Minimize to system tray when closing window; application continues running in background |
| **Exit Directly** | Exit application directly when closing window |
| **Ask for Action** | Pop up dialog to ask each time (default) |

### Auto Start

| Status | Description |
|--------|-------------|
| On | ModxAI Studio starts automatically when system boots |
| Off | Manual startup required (default) |

### Minimize to Tray

| Status | Description |
|--------|-------------|
| On | Hide to system tray when minimized |
| Off | Remain in taskbar when minimized (default) |

---

## Global Parameters

Open **Settings → General**.

Global Parameters affect the runtime behavior of the application and apply to all feature modules.

### Inference Device

Select the computing device to use for model inference:

| Option | Description |
|--------|-------------|
| **Auto** | Automatically detect and select the best device (default) |
| **CPU Only** | Force use of CPU for inference |
| **GPU Only** | Force use of GPU for inference (GPU environment must be installed) |

> 💡 **Tip**: If GPU environment is installed, "Auto" is recommended; the system will select the best device based on actual GPU conditions.

### Max Tokens

Set the global maximum output token limit:

| Property | Value |
|----------|-------|
| Minimum | 1 |
| Maximum | 8192 |
| Default | 4096 |
| Step | 32 |

> Used to limit the maximum number of tokens output per data item during data processing. If exceeded during training, the excess will be truncated.

### Badwords Sensitivity

Set the strictness level of content filtering:

| Option | Description |
|--------|-------------|
| **Off** | No filtering applied (default) |
| **Low Sensitivity** | Basic filtering; only filters obviously violating content |
| **Strict Mode** | Enhanced safety filtering |
| **Block Mode** | Immediately terminate processing when sensitive words detected |

---

## Theme Settings

ModxAI Studio provides multiple preset themes supporting light and dark modes. Users can freely switch according to preference.

### How to Switch

1. Open **Settings → Appearance** and browse the theme preview cards
2. Click on the theme card you want to use
3. Theme takes effect immediately; selection is automatically saved

---

## Version Updates

Open **Settings → Help**.

The Version Updates section displays current version information and supports online updates:

### Version Information

| Item | Description |
|------|-------------|
| Current Version | Currently installed application version number |
| Latest Version | Latest version number on server (displayed after checking) |
| Last Checked | Time of last update check |

### Update Operations

| Operation | Description |
|-----------|-------------|
| **Check for Updates** | Connect to server to check for new versions |
| **Download Update** | Download new version installer (displayed when new version available) |
| **Install and Restart** | Install downloaded update and restart application (displayed after download complete) |

The main menu can check for, download, and install an update directly. You do not have to open this page first.

> Downloads may be slow or fail for users in mainland China. A network proxy is recommended.

The next launch recovers unfinished work. Scheduled tasks that had not finished have their status restored.

---

## Help & Feedback

Open **Settings → Help**.

The Help & Feedback section provides support entry points:

### Analytics & Improvement

| Status | Description |
|--------|-------------|
| On | Anonymously collect runtime status and error information to help improve the product. No personal content data is uploaded |
| Off | No data collection; does not affect normal usage |

### Quick Access

| Entry | Description |
|-------|-------------|
| **Submit Feedback** | Open feedback form to submit issues or suggestions |
| **Documentation** | Open official online documentation |
| **Terms of Service** | View terms of service |
| **Privacy Policy** | View privacy policy |
| **About** | View application information and version number |

---

## MCP External Tools

Open **Settings → MCP** to use Quick Add or configure a custom connection. After connecting, select the tools an agent role may use. See [MCP External Tools](../mcp/overview.md).

---

## Related Documentation

- [Environment Installation Guide](./environment.md) - Detailed environment installation guide and troubleshooting
