# Quick Start

> v1.0.0
> 2025-12

This guide helps you complete your first AI chat in **5 minutes**.

---

## Step 1: Launch the Application

After installation, double-click the desktop icon to launch ModxAI Studio.

On the first launch, the application will automatically complete the initialization configuration without manual operation.

---

## Step 2: Get a Model

For the first use, you need to import or download a chat model.

### Method 1: Online Download (Recommended)

1. Click **"Model Library"** in the left navigation bar.
2. Click the **"Download"** button in the top toolbar.
3. Select a download source (Users in mainland China are recommended to select **ModelScope**).
4. Select a model in the Chat Model category (e.g., Qwen3-0.6B-GGUF).
5. Select a quantized version (Recommended Q4_K_M, balancing effect and resource usage).
6. Click **"Start Download"** and wait for the download to complete.

> 💡 Model files are large, and download time depends on network speed.

### Method 2: Local Import

If you already have a local model file (.gguf format):

1. Click **"Model Library"** -> **"Import"**.
2. Select the model file.
3. Confirm import information and complete.

---

## Step 3: Load the Model

1. In the **"Chat Models"** tab of the Model Library.
2. Check the model you just downloaded/imported.
3. Click the **"Load"** button at the top.
4. Wait for the model loading to complete (Status changes to "Loaded").

---

## Step 4: Start Chatting

1. Click **"Chat"** in the left navigation bar.
2. Click **"New Chat"** or **directly enter a question**.
3. Enter your question in the input box.
4. Press **Enter** or click the send button.

🎉 Congratulations! You have successfully completed your first AI chat!

---

## Advanced Features

After completing the basic chat, you can explore more features:

| Feature | Description | Documentation |
|---------|-------------|---------------|
| **RAG Retrieval** | Let AI answer questions based on your knowledge base | [AI Chat](../features/chat/overview.md) |
| **Image Generation** | Generate images using SD models | [SD Chat](../features/sdchat/overview.md) |
| **Data Processing** | Convert documents into training data | [Data Processing](../features/dataset/overview.md) |
| **Model Training** | Fine-tune your exclusive model | [Model Training](../features/training/overview.md) |

---

## FAQ

### Model download is very slow?

- Try switching the download source (Hugging Face ↔ ModelScope).
- Check network connection.

### Model loading failed?

- Check if system memory is sufficient.
- Try selecting a smaller quantized version (e.g., Q2_K).

### No response in chat?

- Confirm the model has been successfully loaded (Status shows "Loaded").
- Check the system resource monitor at the bottom of the page.

---

## Next Steps

- [Model Library](../features/models/overview.md) - Learn complete model management features.
- [AI Chat](../features/chat/overview.md) - Explore advanced features like RAG and Multimodal.
- [Global Settings](../features/settings/overview.md) - Configure runtime environment to unlock more features.
