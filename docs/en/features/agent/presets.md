# Preset Management

> v1.1.11
> 2026-11

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
- Instruction settings (global instruction file path, search ignore file, personal preferences)
- Runtime policy, including the auto-approval (YOLO) toggle

---

## Saving a Preset

1. Open **Runtime Settings** at the top right of the agent page and adjust the parameters. Configure and save instruction files, search ignore files, and user preferences under **Settings → Agent**.
2. Click **Save Preset** (disk icon) at the top of the parameter panel.
3. Enter a name. Expand **Runtime Policies** to check automatic approval if needed.
4. Click **Save Preset**.

The built-in default preset cannot be deleted.

## Loading a Preset

1. Open **Runtime Settings** and select a preset from the dropdown at the top of the panel.
2. Click **Load Preset** beside the dropdown.
3. Check the loaded parameters and automatic approval setting before sending a task.

Loading replaces the current configuration and restores the preset’s global instruction file path, search ignore file, and user preferences. Save it as a preset first if you need to keep it. You can delete non-default presets from the dropdown after confirming the deletion.
