# Global Settings

> v1.0.0
> 2025-12

This guide introduces how to use the **Settings** feature in ModxAI Studio, helping you complete application configuration, environment installation, theme switching, and license activation.

## Table of Contents

- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Environment Configuration](#environment-configuration)
- [General Settings](#general-settings)
- [Global Parameters](#global-parameters)
- [Theme Settings](#theme-settings)
- [Version Updates](#version-updates)
- [License](#license)
- [Help & Feedback](#help--feedback)
- [Related Documentation](#related-documentation)

---

## Feature Overview

The Settings module is the core configuration center of ModxAI Studio, providing global configuration capabilities at the application level. Key features:

- **One-Click Environment Installation**: Reduces configuration barriers by automatically detecting hardware and recommending optimal environments
- **Differential Dependency Loading**: Install on demand; unused features don't install dependencies, saving disk space
- **Sandboxed Dynamic Loading**: Isolated environments, loaded only when needed to avoid conflicts
- **Multi-Language Support**: Supports Chinese and English interface switching
- **Theme Switching**: Multiple preset themes with light/dark mode support
- **License Management**: Supports lifetime and subscription license activation

---

## Interface Layout

The Settings page uses a card-based vertical layout, organized by functional sections:

| Section | Description |
|---------|-------------|
| **Environment Configuration** | System information viewing, training environment installation and management |
| **General Settings** | Language, close behavior, startup settings, and other window behavior configurations |
| **Global Parameters** | Inference device, max tokens, content filtering, and other runtime parameters |
| **Theme Settings** | Application appearance theme selection |
| **Version Updates** | Current version info, update checking, download and installation |
| **License** | License status viewing and registration code activation |
| **Help & Feedback** | Documentation links, feedback submission, privacy policy, etc. |

---

## Environment Configuration

Environment Configuration is one of the **core highlights** of ModxAI Studio, designed to lower the barrier for environment setup.

### Design Philosophy

- **Zero-Barrier Installation**: Users don't need to manually configure Python, PyTorch, or other complex environments
- **Differential Loading**: Install dependencies on demand; training packages only installed when training features are needed
- **Sandbox Isolation**: Completely independent environment, not dependent on system Python, avoiding conflicts
- **Resume Support**: Installation can continue after interruption; downloaded packages are retained

### System Information

The Environment Configuration section automatically detects and displays current system information:

| Item | Description |
|------|-------------|
| System | Operating system type (Windows) |
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
| Memory | 8GB or more recommended |
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

General Settings configure the basic behavior of the application:

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

1. Browse theme preview cards in the Theme Settings section
2. Click on the theme card you want to use
3. Theme takes effect immediately; selection is automatically saved

---

## Version Updates

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

---

## License

The License section is used to view authorization status and activate registration codes.

### License Types

| Type | Description |
|------|-------------|
| **Free Version** | Advanced features consume monthly free quota |
| **Subscription** | Monthly/yearly subscription with unlimited access to all features |
| **Lifetime** | One-time purchase, permanent use |

### Activation Method

1. Enter registration code in the input box
2. Click the "Activate" button
3. Authorization status displayed after successful activation

### Status Description

| Status | Description |
|--------|-------------|
| Activation Successful | Valid authorization; unlimited access to all features |
| Expired | Subscription has expired; renewal required |
| Authorization Invalid | Registration code has been revoked; please contact support |

> For detailed authorization guide, please refer to [License Activation Guide](./license.md)

---

## Help & Feedback

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

## Related Documentation

- [Environment Installation Guide](./environment.md) - Detailed environment installation guide and troubleshooting
- [License Activation Guide](./license.md) - License type descriptions and activation process

