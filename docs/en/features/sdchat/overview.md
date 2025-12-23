# SD Chat - Overview

> v1.0.0
> 2025-12

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Introduction](#feature-introduction)
- [Prerequisites](#prerequisites)
- [Interface Layout](#interface-layout)
- [Generation Modes](#generation-modes)
- [Basic Operations](#basic-operations)
    - [Create Conversation](#create-conversation)
    - [Send Prompt](#send-prompt)
    - [Attachment Upload](#attachment-upload)
    - [Conversation Management](#conversation-management)
- [Parameter Configuration](#parameter-configuration)
    - [Basic Parameters](#basic-parameters)
    - [Negative Prompt Templates](#negative-prompt-templates)
    - [Image Saving](#image-saving)
    - [General Advanced Parameters](#general-advanced-parameters)
    - [Image-to-Image Specific Parameters](#image-to-image-specific-parameters)
    - [Video Generation Specific Parameters](#video-generation-specific-parameters)
- [Message Operations](#message-operations)
- [Progress Monitoring](#progress-monitoring)
- [Favorite Data](#Favorite-data)
- [Important Notes](#important-notes)

---

## Environment Requirements

**This feature requires installing GPU or CPU environment dependencies in the settings.**

SD Chat uses Stable Diffusion models for image and video generation. Before using, ensure:

1. Successfully loaded a **Stable Diffusion (Image Generation)** type model in **Model Library**
2. The model's supported generation modes (text-to-image/image-to-image/video) will affect available features

> **GPU Acceleration Libraries (Important)**
>
> - **NVIDIA (Recommended; Required for SD)**:
>   - If an NVIDIA discrete GPU is detected, it is recommended to install the corresponding CUDA acceleration libraries via "Model Library → ⚡ button → Install GPU Acceleration Libraries". Chat/multimodal models perform best on NVIDIA+CUDA; Stable Diffusion (SD, image generation) currently only supports CUDA, so to use SD features, CUDA acceleration libraries must be installed first.
>   - After installing or updating the acceleration libraries, there is no need to restart the application. The changes will take effect the next time a model is loaded.
> - **AMD (Limited)**:
>   - AMD GPUs can use built-in Vulkan acceleration to run chat/multimodal models (no additional installation required), but SD inference currently does not support AMD/Vulkan, so SD features cannot be used on AMD GPUs.
> - **No Discrete GPU / Integrated GPU**:
>   - If no discrete NVIDIA GPU is detected, the "Install GPU Acceleration Libraries" button will not be shown. Chat models can run on CPU or platform Vulkan (in supported scenarios), but performance will be limited.
---

## Feature Introduction

SD Chat is a specialized conversational interface for Stable Diffusion image/video generation, supporting:

- **Text-to-Image**: Generate images from text prompts
- **Image-to-Image**: Transform images based on input image and prompts
- **Video Generation**: Generate dynamic video content

Each conversation can independently configure generation parameters (resolution, sampling steps, CFG, seed, etc.), supports multi-turn conversational generation for easy debugging and effect comparison.

---

## Prerequisites

1. **Must Load SD Model First**
     - Go to **Model Library** page
     - Load a model in the **Stable Diffusion (Image Generation)** tab
     - After successful model loading, SD Chat page will automatically unlock

2. **Understand Model Capabilities**
     - Different SD models support different features:
         - Base models: Usually support text-to-image
         - Image-to-image models: Support text-to-image + image-to-image
         - Video models: Support video generation (may include text-to-image/image-to-image)
     - Check model details in Model Library to understand supported capabilities

---

## Interface Layout

SD Chat interface uses a left-right dual-column layout:

### Left Sidebar (Conversation List)
- **New Conversation** button: Located at top
- **Conversation List**: Grouped by time (Today/Yesterday/Last 7 Days/Last 30 Days/Earlier)
    - Shows conversation thumbnail (first generated image)
    - Shows conversation title (automatically generated from first prompt)

### Right Panel (Chat Area)
- **Message List**: Displays user prompts and AI-generated images/videos
- **Progress Bar**: Shown below message list during generation
- **Input Box**: Bottom area for entering prompts
- **Attachment Button**: Left of input box (only shown when image-to-image or video is supported)
- **Parameters Button**: Bottom toolbar of input box, click to open parameters drawer
- **Generate Button**: Bottom right of toolbar

> **Prompt When Model Not Loaded**: Right panel shows "Go to Model Library to Load <Image Generation Model>" button, click to jump to Model Library.

---

## Generation Modes

**Quota Consumption**: Each successful generation consumes 1 "data" quota (counted from first generation after loading model).

SD Chat automatically supports the following modes based on loaded model capabilities:

| Mode | Description | Attachment Requirement | Generated Content |
|------|-------------|------------------------|-------------------|
| **Text-to-Image** | Generate images from text prompts | No attachment area shown | Images (1-16, depends on `Batch Count` parameter) |
| **Image-to-Image** | Transform images based on input image | Optional image upload (automatically falls back to text-to-image if not uploaded) | Images (1-16) |
| **Video Generation** | Generate dynamic videos | Optional initial frame image upload | Video file + video frame images |

> **Automatic Mode Switching**:
> - Text-to-image mode: No attachment button shown
> - Image-to-image/video mode: Attachment button shown, supports image upload (JPG, PNG formats)

---

## Basic Operations

### Create Conversation

1. Click **New Conversation** button at top of left sidebar
2. System will create a new conversation and automatically switch to it
3. Conversation title will be automatically generated after first prompt is sent (based on prompt content)

> **Note**: Only one empty conversation can exist at a time, must use or delete empty conversation before creating new one.

### Send Prompt

1. Enter prompt in input box
2. Click **Generate** button (or press `Ctrl+Enter`)
3. System will immediately show user message and start generation
4. After generation completes, AI message will display generated images/videos

> **Tips**:
> - Prompts support both English and Chinese, but English prompts usually work better
> - More specific and detailed prompts produce results closer to expectations
> - Overly long prompts may lead to generation failure; it is recommended to keep prompts within 500 characters

### Attachment Upload

**Attachment functionality only shown in image-to-image or video mode.**

1. Click **attachment icon** (link icon) on left of input box (shows red badge when attachments present)
2. Two ways to add images:
     - **Click upload area**: Select local image file
     - **Drag and drop**: Drag image directly to upload area
3. Supported image formats: JPG, JPEG, PNG
4. Only first uploaded image is used each time
5. Click delete button on attachment card to remove

> **Image-to-Image Mode Notes**:
> - Uploaded image will be used for image-to-image transformation
> - Not uploading image automatically falls back to text-to-image mode

> **Video Mode Notes**:
> - Attachment is optional, not uploading image uses pure text-to-video mode
> - Uploaded image becomes initial frame of video

### Conversation Management

#### Select Conversation
- Click conversation item in left list to switch

#### Rename Conversation
1. Right-click conversation item (or click `···` menu on right of conversation)
2. Select **Rename**
3. Enter new title (maximum 80 characters)
4. Click **Confirm**

> **Auto Title**: After first prompt is sent, system automatically generates title from prompt (maximum 30 characters). After manual rename, no longer auto-updates.

#### Delete Conversation
1. Right-click conversation item
2. Select **Delete**
3. Confirm deletion (will delete conversation along with all messages and generated image records)

---

## Parameter Configuration

Click **Parameters button** (slider icon) on bottom toolbar of input box to open parameters drawer.

### Basic Parameters

Following parameters apply to all generation modes:

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| **Width** | 768 | 64-2048 (step 64) | Width of generated image (pixels) |
| **Height** | 768 | 64-2048 (step 64) | Height of generated image (pixels) |
| **Sample Steps** | 20 | 1-200 | Generation iteration count, more steps = higher quality but slower |
| **CFG Scale** | 7.0 | 0-30 (step 0.5) | Prompt adherence degree, higher = stricter prompt following |
| **Seed** | Random | 0-99999999 | Controls generation randomness, same seed produces similar results |

> **Resolution Recommendations**:
> - Standard: 512×512, 768×768
> - Widescreen: 1024×576, 1280×720
> - Portrait: 576×1024, 720×1280
> - Higher resolutions consume more VRAM and time

### Negative Prompt Templates

Negative prompts exclude unwanted elements (such as "low quality, blurry, deformed").

**Use Template**:
1. Click **dropdown** above negative prompt input box
2. Select preset template from list (e.g., "General Negative Prompts")
3. Template content automatically fills input box

**Save Template**:
1. Edit negative prompt content
2. Click **Save As** button
3. Enter template name
4. Click **Confirm**

**Delete Template**:
1. First select template to delete
2. Click **delete icon** (trash can)
3. Confirm deletion

> **Note**: Template data stored in application's local database, deleted templates cannot be recovered. Uninstalling app does not delete database.

### Image Saving

| Parameter | Default | Options | Description |
|-----------|---------|---------|-------------|
| **Save Directory** | Empty (use temp directory) | Any local path | Specify image save location, leave empty to save to system temp directory |
| **Image Format** | PNG | PNG, JPEG, WebP | PNG lossless but large, JPEG lossy but small |
| **Image Quality** | 85 | 1-100 | Only applies to JPEG/WebP formats, PNG format auto-disables this option |

> **Video Frame Note**: Video generation frame images always use PNG format (lossless) to ensure video quality.

### General Advanced Parameters

Expand **General Advanced Parameters** collapse panel to view following options:

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| **Sample Method** | default | euler, euler_a, ddim, dpmpp_2m, etc. | Different algorithms vary in speed and quality |
| **Scheduler** | default | normal, karras, exponential, etc. | Controls noise scheduling in sampling process |
| **Guidance** | 3.5 | 0-30 (step 0.5) | Classifier-free guidance strength, affects generation details |
| **Batch Count** | 1 | 1-16 | Number of images to generate at once (not supported in video mode) |
| **Upscale Factor** | 1 | 1-4 | Auto-upscale images after generation |
| **CLIP Skip** | -1 | -1 to 12 | Skip last N CLIP layers, -1 means no skip |

> **Sample Method Recommendations**:
> - **euler**: Fast, suitable for quick preview
> - **euler_a**: Good stability, suitable for most scenarios
> - **dpmpp_2m**: High quality but slower

### Image-to-Image Specific Parameters

Expand **Image-to-Image Mode Parameters** collapse panel (only shown in image-to-image mode):

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| **Strength** | 0.55 | 0-1 (step 0.01) | Controls similarity between generated image and original, 0=identical, 1=complete redraw |
| **VAE Tiling** | Off | On/Off | Reduces VRAM usage, recommended when processing large images |

> **Strength Parameter Guide**:
> - **0.3-0.5**: Fine-tune details, preserve original subject
> - **0.5-0.7**: Balanced changes, recommended range
> - **0.7-1.0**: Major changes, close to regeneration

### Video Generation Specific Parameters

Expand **Video Mode Parameters** collapse panel (only shown in video mode):

| Parameter | Default | Description |
|-----------|---------|-------------|
| **Video Frames** | Equal to sample steps | Total frames in generated video, controlled by `Sample Steps` parameter (1-200 frames) |

> **Video Parameter Notes**:
> - In video mode, `Sample Steps` controls both generation quality and video length
> - More steps = longer video but longer generation time
> - Recommended 20-60 frames (approximately 1-4 second video, depending on FPS)

---

## Message Operations

### User Message Operations

Hover over user message to show action buttons (right floating toolbar):

- **Edit**: Modify prompt content (does not regenerate)
- **Copy**: Copy prompt to clipboard
- **Delete**: Delete this message (does not affect other messages)
- **Resend**: Regenerate using same prompt

> **Resend Notes**:
> - Preserves original message's generation mode (text-to-image/image-to-image/video)
> - Image-to-image mode requires original message contains source image, otherwise auto-falls back to text-to-image
> - If model not currently loaded, shows "Stable Diffusion model not loaded" prompt

### AI Message Operations

Hover over AI message to show:

- **Save Image As**: Save generated image locally
- **Copy**: Copy message content (if text present)
- **Delete**: Delete this message

### Image Saving

1. Click **Save Image As** button (download icon)
2. System opens file save dialog
3. Default filename format: `{conversation_title}_{timestamp}.{format}`
4. Select save location and confirm

> **Batch Save**: If multiple images generated at once (batch generation), each image must be saved separately.

### Video Operations

Video messages display:
- **Inline Player**: Play directly in chat interface
- **Open in System Player** button: Open with system default player

---

## Progress Monitoring

### Progress Bar Display

During generation, progress bar shown below message list:
- **Progress Percentage**: Current step / total steps
- **Stop Generation** button: Click to cancel current generation task

### Stop Generation

1. Click **Stop Generation** button
2. System sends cancel request (button changes to "Canceling...")
3. After successful cancel, progress bar disappears, partially generated images not saved

> **Note**: Cancel operation irreversible, consumed generation time and resources cannot be recovered.

---

## Favorite Data

### Like & Favorite (Reserved Feature)

**Book icon** floating button in bottom right corner for viewing Favorite data favorites. Current version does not yet support liking in SD Chat, future versions will support favoriting generation results for training image-text models.

---

## Important Notes

### Model Requirements

1. **Must Load Model First**
     - SD Chat completely depends on Stable Diffusion model
     - When model not loaded, can only view conversation history, cannot send new messages

2. **Model Capability Detection**
     - System automatically detects model's supported capabilities (text-to-image/image-to-image/video)
     - Unsupported features automatically hidden (e.g., text-to-image model does not show attachment button)

3. **Model Switching**
     - After switching models, new conversations use new model's capabilities
     - Historical conversations retain original model's generation records

### Parameter Settings

1. **Parameter Independence**
     - Each conversation has independent parameter configuration
     - Modifying parameters only affects current conversation's subsequent generations
     - New conversations use default parameters

2. **Parameter Persistence**
     - Parameter settings automatically saved to browser local storage
     - Clearing browser data loses custom parameters

3. **Reset Button**
     - **Reset icon** (refresh) on right of each parameter restores parameter's default value

### Attachment Limitations

1. **Image Format**
     - Only supports JPG, JPEG, PNG formats
     - Does not support GIF, WebP, BMP, etc.

2. **Image Size**
     - Recommended single image not exceed 10MB
     - Oversized images may cause excessive memory usage

3. **Attachment Count**
     - Only first uploaded image used each time
     - When uploading multiple images, system ignores all but first

### Generation Limitations

1. **Concurrency Limit**
     - Only one generation task at a time
     - Cannot send new messages during generation

2. **Resource Usage**
     - High resolution + high steps consume significant VRAM and time
     - Recommended parameter adjustments based on hardware:
         - **4GB VRAM**: 512×512, steps 20-30
         - **8GB VRAM**: 768×768, steps 30-50
         - **12GB+ VRAM**: 1024×1024, steps 50+

3. **Video Generation Special Notes**
     - Video generation takes much longer than image generation (typically 5-10x)
     - Recommended to test with low frame count first (20 frames), then increase after confirming effect

### Data Management

1. **History Records**
     - Conversations and messages persistently stored in local database
     - Uninstalling app does not delete local database

2. **Image Storage**
     - Generated images saved to system temp directory by default
     - Important images should be saved to specified location promptly
     - Setting **Save Directory** parameter specifies default save location

3. **Lazy Loading**
     - Conversation list supports lazy loading (scroll to bottom auto-loads more)
     - Message list supports lazy loading (scroll to top loads history messages)

### Common Issues

**Q: Why does clicking "Generate" button not respond?**

A: Check the following:
1. Is SD model loaded (interface shows "Go to Model Library to Load <Image Generation Model>" prompt when not loaded)
2. Is prompt empty
3. Is there an ongoing generation task

**Q: How to improve unsatisfactory image quality?**

A: Try these adjustments:
1. Increase sample steps (recommended 30-50)
2. Adjust CFG scale (recommended 7-12)
3. Use more detailed prompts
4. Add negative prompts to exclude unwanted elements
5. Try different sample methods

**Q: Why does image-to-image result differ too much from original?**

A: Lower **Strength** parameter (recommended 0.3-0.6).

**Q: What to do if video generation fails or gets stuck?**

A:
1. Reduce video frame count (recommended 20-40 frames)
2. Lower resolution (recommended 512×512 or 768×768)
3. Check if VRAM is sufficient
4. Click "Stop Generation" and retry

**Q: How to reproduce previously generated images?**

A:
1. Check generation parameters of historical message (may display when hovering over image)
2. Use same prompt and negative prompt
3. Set same random seed
4. Ensure other parameters (resolution, steps, CFG, etc.) also match

**Q: Why are some parameters grayed out and unavailable?**

A:
- **Image Quality**: Only available for JPEG/WebP formats
- **Batch Count**: Not available in video mode
- **Image-to-Image Parameters**: Only available in image-to-image mode
- **Video Parameters**: Only available in video mode

---

## Next Steps

- Learn how to import and load SD models in [Model Library](../models/overview.md)
- Check [Prompt Generation](../chat/prompt-generation.md) feature to learn how to optimize prompts
- Explore [Favorite Data](../chat/personalized-data.md) feature to favorite quality generation results

