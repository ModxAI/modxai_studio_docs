# Training Evaluation Guide

> v1.1.11
> 2026-11

Use **Self Improvement → Training → Evaluate** to compare the base model and checkpoints on fixed benchmarks, inspect scores and individual answers, and keep evaluation history.

## Contents

- [Before you start](#before-you-start)
- [Evaluation workflow](#evaluation-workflow)
- [Built-in benchmarks](#built-in-benchmarks)
- [Evaluation parameters](#evaluation-parameters)
- [Read the results](#read-the-results)
- [Evaluation history](#evaluation-history)
- [Important notes](#important-notes)
- [Evaluation and interactive testing](#evaluation-and-interactive-testing)

## Before you start

- Install a [runtime environment](../settings/environment.md) that supports inference with training models, and have a finished training task.
- Keep the task's base model, tokenizer, and checkpoint files. LoRA checkpoints need their corresponding base model. GGUF files cannot be evaluated here.
- Allow sufficient memory or GPU memory. Adding models or benchmarks increases the work required.
- Prepare datasets and scoring components as prompted. Missing resources require an internet connection to download. Once resources are ready, evaluation runs using local models.

The task selector includes completed, stopped, failed, and error tasks. Only available checkpoints can be selected; a task record does not establish that its weights are complete.

## Evaluation workflow

1. Open **Self Improvement → Training → Evaluate** and select a **Training Task**.
2. Select one or more **Checkpoints**, or click **Select All**. Keep **Include the Base Model** enabled to compare against the original model. You can leave checkpoints empty to evaluate only the base model.
3. Choose one or more **Benchmarks**. Each selected benchmark shows its question count for the current scale and its resource status.
4. Open **Evaluation Parameter Configuration**. Set the scale, maximum output tokens, execution device, and optional additional thinking, then click **Apply**.
5. If the selected scale needs a dataset, click its download icon. IFEval also needs scoring components; select **Prepare Scoring Components** when prompted. Wait for **Ready**. Retry failed downloads, or click a progress icon to cancel a download or verification.
6. Click **Check and continue**. The app checks resources, model files, the environment, and question lengths. You can cancel while checking. If a check fails, resolve the reported issue and check again.
7. When **Ready to start** appears, click **Start Evaluation**. Passing checks does not itself start evaluation. Changing the configuration requires another check.
8. Inspect progress and completed results on the results page. You can leave the page and return through **View progress** or **Evaluation History**.

To stop, return to evaluation configuration and click **Stop**. Wait for the status to become **Stopped**. If stopping fails, retry as prompted; requesting a stop is not the same as completion.

## Built-in benchmarks

| Benchmark | Measures | Overview score |
|-----------|----------|----------------|
| IFEval | Compliance with formatting and instruction constraints | Proportion strictly satisfying all requirements in a question |
| MMLU-Pro | Multiple-choice professional knowledge and reasoning | Answer accuracy |
| GSM8K | Grade-school math problems | Final numeric-answer accuracy |
| HellaSwag | Choosing a natural continuation from candidate endings | Length-normalized candidate-selection accuracy |

The current evaluation page uses these built-in benchmarks. It has no custom `prompt/completion` sample-file input. Use [Test](./test.md) for your own questions.

## Evaluation parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Evaluation Scale | Quick | Quick, Extended, or Full; check the question count beside each benchmark |
| Maximum Output Tokens | 2048 | Maximum generated answer length, from 1 to 32768 |
| Execution Device | Inherit Training Task | Inherit, Auto (CPU/CUDA), CPU, or GPU (CUDA) |
| Additional Thinking | Off | Applies only when supported by the model and its template |

Use **Quick** to check the workflow, **Extended** for more questions, and **Full** for the complete fixed dataset. Smaller samples can vary more and should not be treated as a final judgment of model capability.

This page does not offer temperature, Top P, Top K, random seed, or custom sampling-ratio controls. Evaluation settings are separate from interactive test settings.

Questions plus the output budget must fit the model's context capacity. If the output budget is too large, lower **Maximum Output Tokens** and check again. If the question itself is too long, choose a model with enough context capacity.

## Read the results

### Score Matrix

Rows are models and columns are benchmarks. Scores are percentages:

- Click a score to inspect questions, model answers, reference answers or requirements, and judgments.
- Use **Show Differences** to filter samples that did not meet requirements. Refresh or load more when needed.
- **Highest for This Benchmark** compares only complete results without execution failures for that benchmark. It is not an overall ranking across benchmarks.
- When complete comparable results include the base model, the matrix shows the change from it in percentage points.
- Incomplete cells also show completion counts. An execution failure is different from an incorrect model answer.

### Benchmark Trend

Choose a benchmark to compare complete scores across models in this run. Incomplete results and results with execution failures are excluded from the trend.

### Compare Two Models

1. Select the first model, second model, and benchmark.
2. Click **Compare Two Models** to inspect the same question side by side.
3. Filter by **Second Model Improved**, **First Model Better**, **Both Correct**, **Both Incorrect**, or **Unavailable**, or enable **Show Differences**.

The comparison uses models from this run. Different wording when both answers are wrong does not establish that one model is better.

## Evaluation history

Open **Evaluation History** to filter runs by training task, status, and date.

| Action | Purpose |
|--------|---------|
| View | Open the run's scores and individual results |
| Copy Configuration | Return its configuration to the evaluation page, check it, and start a new run |
| Continue Evaluation | Available for stopped, failed, or interrupted runs; return to configuration and check again, reusing eligible completed results |
| Recalculate | Return to configuration and evaluate without reusing existing results |
| Export | Download JSONL containing configuration, summaries, and individual results |
| Delete | Confirm deletion of a run that is not active; this does not delete training models or checkpoints, or clear all reusable results |

Normal evaluation may reuse results for the same model, question, and settings. The ready-to-start area reports reusable results and the remaining work. Use **Recalculate** when you want all results generated again.

## Important notes

- Only one evaluation can run at a time. View or stop the active run before starting another.
- Do not directly compare partial scores with complete scores. Completed samples remain accessible after failures or interruptions, and you can continue from history.
- Benchmark scores describe performance on their datasets. They do not guarantee performance on your business tasks, Chinese-language ability, or factual accuracy.
- Keep scale and generation settings consistent when comparing checkpoints, then check your own questions.
- Exports may contain local model paths, questions, and model answers. Review their contents before sharing.

## Evaluation and interactive testing

Evaluation provides fixed-dataset scoring and model comparisons. [Test](./test.md) lets you enter your own questions and inspect answers. Use evaluation to shortlist checkpoints, test them on real tasks, and then choose a model for [quantization](./package.md).

## Related guides

- [Prepare](./prepare.md): create training tasks
- [Monitor](./monitor.md): training loss and logs
- [Test](./test.md): inspect actual answers
- [Quantize](./package.md): export the model
