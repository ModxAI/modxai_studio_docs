# Audio to Text

> v1.1.11
> 2026-11

Transcribe local audio files in batches into timestamped text, with optional subtitle and document exports. Use it for meeting recordings, interviews, and podcasts. This page covers file transcription, not live microphone transcription.

## Before You Start

- Install the required CPU or GPU environment in **Settings → System** and confirm that it is ready. See [Environment Setup](../settings/environment.md).
- Put your `.mp3`, `.wav`, `.m4a`, or `.wma` files in an input folder.
- Prepare a separate output folder. Processing includes subfolders of the input folder, so try a few files first.
- Add a **faster-whisper** model to your model library. Keep the complete model folder, including `model.bin` and either `vocabulary.json` or `tokenizer.json`. Do not download only the weights file.

### Download a Model

1. Open **Model Center → Download**.
2. In **Browse**, select **Audio2Text**, then choose a model, download source, and version.
3. Check the required disk space and save location, then start the download.
4. Check the download and import results in **Downloads**, then confirm that the model appears in **Model Center → STT**.

Downloads started from **Browse** automatically attempt to import the model after file verification. **Repository** downloads and files downloaded elsewhere need a manual import: open **Model Center → STT**, click **Import**, select `model.bin` inside the model folder, check the name and category, and complete the import.

Downloading requires an internet connection. Use the size shown on the download page. Memory use, VRAM use, and transcription speed depend on the model, audio, and device.

## Run a Transcription

1. Open **Self Improvement → Dataset**, then select **MM → Audio2Text**.
2. Select or enter the audio folder in **Input Directory** and set the result location in **Output Directory**.
3. Under **File type**, keep **All supported types** or select only the extensions you want to process.
4. In **Audio2Text Model**, select an imported faster-whisper model.
5. Keep **Model Loading**, **Transcribe**, and **Output** selected. To change settings, click **…** on the relevant step and save your changes. For a first run, start with the defaults.
6. Click **Start**. In **Monitoring**, check the file count, progress, and logs. The app scans the input folder and its subfolders, then processes files one at a time, starting with the most recently modified.
7. Check the completion or error message. Click **Open** in the success dialog to open the output folder.

If a newly imported model is missing from the selector, leave the Dataset page and return before selecting it.

## Check the Results

Results are saved in your output folder using the audio filename without its extension. For example, `meeting.mp3` produces `meeting.json`.

| Format | What it contains |
| --- | --- |
| JSON | Generated for each completed audio file. Includes recognized text, segment start and end times, and the detected language. Word-level timing is included when enabled. |
| SRT | Optional subtitles with timecodes. |
| HTML | Optional timestamped text for reading in a browser. |
| Markdown (md) | An optional timestamped text list for further editing. |

JSON is generated even if you select only other output formats. Check that the expected files exist, then review the text and timestamps, especially names, technical terms, and noisy sections.

**Watch for filename collisions:** all results go directly into the same output folder. Input subfolders are not recreated. Audio files with the same name in different subfolders, or `meeting.mp3` and `meeting.wav`, write to the same result filenames. Rename them first or process separate batches into different output folders. Existing results with matching names may also be overwritten.

## Settings

These are the initial values for commonly used settings. If you have saved changes before, use the values currently shown in the settings panel.

### Model Loading

| Setting | Initial value | Notes |
| --- | --- | --- |
| Device | `auto` | Options: `auto`, `cuda`, `cpu`. Automatic mode checks the device and runtime. The app may use CPU if CUDA is unavailable. |
| Compute Type | `float16` | Options: `float32`, `float16`, `int8`, `int8_float16`, `int8_float32`. CPU supports `float32`, `int8`, and `int8_float32`; incompatible CPU settings are changed to `float32`. |
| CPU Threads | `4` | Advanced setting, range 1–32. |
| Workers | `1` | Advanced setting, range 1–8. |
| Device Index | `0` | Advanced setting, range 0–7, for selecting a CUDA device. |

To apply new device or compute settings to a model that is already loaded, finish the task, unload it in **Model Center → STT**, then start a new task.

### Transcribe

| Setting | Initial value | Notes |
| --- | --- | --- |
| Beam Size | `5` | Range 1–10. A higher value increases decoding work but does not guarantee better accuracy. |
| Language | `auto` | Detect automatically, or select Chinese, English, Japanese, Korean, Spanish, French, German, or Russian. |
| VAD Filter | On | Voice activity detection selects speech segments. If speech is missing, try again with this off. |
| Word Timestamps | Off | Advanced setting. Adds word-level timing to JSON results. |
| Min Silence Duration | `2000` ms | Advanced setting, range 100–5000 ms. Used when VAD is enabled. |

### Output

**Output Formats** initially selects `json`. You can also select `srt`, `html`, and `md`. See [Check the Results](#check-the-results) for file contents and naming.

## Stop and Run Again

- Click **Stop Processing** and wait for the interface to confirm that the task has stopped before starting another. Stopping may take time.
- Completed files remain in the output folder. The current audio may leave a `.transcribing.json` temporary file, which is not a complete result. After writing the final JSON normally, the app attempts to remove the corresponding temporary file.
- Transcription cannot resume from a temporary file or an interrupted position. Clicking **Start** again rescans and processes matching input files; completed files are not skipped automatically.
- To retry only failed or unfinished files, put them in a separate input folder and use a new output folder to protect results you have already checked.

## Notes

- **Finishing a task does not unload the model.** After transcription completes or is stopped during processing, the loaded model remains available. To release resources, wait until the task ends, then open **Model Center → STT** and click **Unload**.
- **Check errors before treating the batch as complete.** Other files may continue processing when one fails. Compare the logs with the output folder to identify files that need another attempt.
- **Choose settings that fit your device.** If memory or VRAM is insufficient, try a smaller model, or switch to CPU with a compatible compute type. Model download size alone does not tell you how much memory it needs to run.

## FAQ

### What if the model list is empty or the model fails to load?

Confirm that the model is imported under **Model Center → STT**. Check that its path still exists, the download is complete, and `model.bin` is in the same folder as `vocabulary.json` or `tokenizer.json`. Return to the Dataset page and select the model again.

### What if no files are found?

Check the input folder and **File type** filter. The folder must contain audio with the selected extensions, rather than videos or archives. Try playing the audio to confirm that it is readable.

### What if the transcript is empty or misses speech?

Check that the recording contains clear speech and that the language setting is appropriate. When VAD produces very few segments, the app attempts another transcription with VAD disabled; this does not guarantee a complete result. If the problem remains, turn VAD off manually and retry a single file, then review the output.

## Related Guides

- [Dataset Overview](./overview.md)
