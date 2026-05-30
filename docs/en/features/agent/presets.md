# Preset Management

> v1.1.0
> 2026-05

Preset management allows you to save the complete parameter configuration of the workbench and restore it quickly when needed. Ideal for switching configurations between different task types.

## Table of Contents

- [What is a Preset?](#what-is-a-preset)
- [Saving a Preset](#saving-a-preset)
- [Loading a Preset](#loading-a-preset)

---

## What is a Preset?

A preset is a **complete snapshot** of the workbench parameters, including:

- All role parameter configurations (model, prompt, inference parameters, etc.)
- Advanced parameter settings
- Workspace directory and Skills directory
- Instruction settings (global instruction file path, personal preferences)

---

## Saving a Preset

1. Adjust all parameters in the right configuration panel.
2. Click the **Preset Management** button.
3. In the preset dialog, click **Save Current Configuration**.
4. Enter a preset name.
5. Confirm to save.

The system includes a built-in default preset (non-deletable) that is automatically created on first use after installation.

---

## Loading a Preset

1. Click the **Preset Management** button.
2. Find the target preset in the preset list.
3. Click on the preset and select **Load**.
4. After confirmation, the current workbench parameters will be replaced by the preset configuration.

Loading a preset overwrites the current session's configuration. It is recommended to save the current configuration before loading a preset.
