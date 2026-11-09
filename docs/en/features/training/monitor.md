# Training Monitoring Guide

> v1.1.11
> 2026-11

Use **Self Improvement → Training → Monitor** to inspect training progress, metric curves, and logs, or review a task that has already ended.

## Contents

- [Before you start](#before-you-start)
- [View a training task](#view-a-training-task)
- [Interface layout](#interface-layout)
- [Metric charts](#metric-charts)
- [Chart settings](#chart-settings)
- [Accumulated chart data](#accumulated-chart-data)
- [Training logs](#training-logs)
- [Real-time monitoring](#real-time-monitoring)
- [History review](#history-review)
- [Usage suggestions](#usage-suggestions)

## Before you start

Create and start a task in [Prepare](./prepare.md). You can also select an existing task directly from the monitor. A task without recorded training metrics has no curves to inspect.

## View a training task

1. Open **Self Improvement → Training → Monitor**. Starting training successfully from Prepare also opens this page automatically.
2. Choose the task in the selector at the top.
3. A running task uses real-time monitoring. A task that is not running loads its saved records in history mode. The mode changes automatically.
4. Inspect progress, current metrics, and curves. Expand **Other Charts** for additional metrics and scroll down to **Training Logs**.
5. To stop training, return to [Prepare](./prepare.md), click **Stop Training**, and wait for the status to change.

Progress and estimated time describe the run, not the model's quality. Check the final task status and logs to determine whether training ended normally.

## Interface layout

| Area | Contents |
|------|----------|
| Task selector | Searchable task names and statuses |
| Progress | Current step, total steps, percentage, and estimated time remaining |
| Current metrics | Current loss, learning rate, and processing speed (SPL/s, or samples per second) |
| Main charts | Training loss and perplexity |
| Other charts | Gradient norm, learning rate, evaluation loss, and training speed |
| Training logs | Timestamped training events and metric records |

## Metric charts

| Metric | How to read it |
|--------|----------------|
| Training loss | Look at the overall trend; a single fluctuation does not establish a failure |
| Perplexity | Reflects uncertainty when predicting training text; compare under the same data and settings rather than using a universal target |
| Gradient norm | Look for sustained unusual changes and investigate the learning rate and data |
| Learning rate | Check the warmup and scheduling pattern |
| Evaluation loss | Comes from evaluation during training and requires a separate evaluation file and an active evaluation strategy |
| Training speed | Samples per second; affected by the model, sequence length, batch size, device, and saving operations |

If the evaluation-loss chart is empty, check the evaluation file and strategy in the training parameters. This loss is different from the benchmark scores in [Evaluate](./evaluate.md).

## Chart settings

Click the gear beside the task selector to open chart settings:

- **Smoothing strength**: `0%` shows raw data. Higher values smooth the trend.
- **Moving window**: a larger window smooths local fluctuations more.
- **EMA coefficient**: a higher value responds more quickly to new data.
- Click **Apply** to confirm, or use **Reset Default** to restore defaults.

Use the mouse wheel to zoom, then drag to pan. Smoothing changes the display only. To investigate a spike, set smoothing to `0%` and compare it with the logs.

## Accumulated chart data

History is saved per training task. Restarting the same task may leave records from several runs together, with step numbers starting again. This does not mean training resumed from a checkpoint.

Create separate tasks when comparing different parameter sets. Leaving the page does not stop training; select the task again when you return.

## Training logs

Logs include training start, training steps, evaluation, checkpoint saves, and completion events. The live list shows up to the latest `200` entries. History review loads saved training records.

These are training events and metrics, not necessarily a complete copy of every terminal message. For a failed task, also read the failure message. Deleting a training task deletes its log records.

## Real-time monitoring

Running tasks update charts as new metrics arrive. The **Logging steps** setting controls the interval, with a preset of every `10` steps. This is not a fixed once-per-second update rate.

Live curves show up to the latest `400` points. Metrics may pause while the model loads, data is prepared, or checkpoints are saved. Check logs and status before treating an unchanged chart as a failure.

## History review

Select a task that is not running to inspect saved metrics and events, including completed, stopped, or failed tasks. History charts show up to the latest `4000` points, so a chart may not contain the full history.

If there are no curves, check whether the task reached training and produced metrics. If history fails to load, select the task again to retry.

## Usage suggestions

- Compare trends using the same data and settings. Fixed loss or gradient thresholds do not apply to every model.
- If training loss falls while evaluation loss keeps rising, investigate overfitting before adjusting epochs, learning rate, or data.
- Use benchmark evaluation and interactive testing to assess the model's actual capabilities.

## Related guides

- [Prepare](./prepare.md): parameters and stopping training
- [Evaluate](./evaluate.md): compare checkpoints
- [Test](./test.md): inspect actual answers
- [Quantize](./package.md): export the model
