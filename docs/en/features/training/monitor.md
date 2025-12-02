# Training Monitor Module Details

> v1.0.0
> 2025-12

> 📌 **Module Positioning**: Real-time monitoring of training progress, viewing historical training data, and analyzing training status through visual charts.

## Table of Contents
- [Overview](#overview)
- [Interface Layout](#interface-layout)
- [Metric Charts](#metric-charts)
- [Chart Data Accumulation Mechanism](#chart-data-accumulation-mechanism)
- [Training Log](#training-log)
- [Real-time Monitoring Mode](#real-time-monitoring-mode)
- [History Review Mode](#history-review-mode)
- [Usage Suggestions](#usage-suggestions)
- [Next Steps](#next-steps)

---

## Overview

The Monitor module provides two working modes:

| Mode | Trigger Condition | Description |
|------|----------|------|
| **Real-time Monitoring** | Select a running task | Receive training metrics in real-time, charts update dynamically |
| **History Review** | Select a completed/stopped task | Load historical training logs, view complete training curves |

The system automatically switches modes based on the selected task status, no manual setting required.

---

## Interface Layout

### Task Selector

The task selector at the top of the page is used to select the training task to monitor:

- **Dropdown List**: Displays all training tasks, including task names and status tags.
- **Status Tag Colors**:
  - 🟢 Green: Running
  - 🔵 Blue: Completed
  - 🟡 Yellow: Pending
  - 🔴 Red: Failed/Error
  - ⚪ Gray: Stopped

### Training Status Panel

Displays key progress information of the current training:

| Metric | Description |
|------|------|
| Current Steps | Number of training steps completed |
| Total Steps | Estimated total steps of the training task |
| Training Progress | Completion percentage |
| Estimated Remaining Time | Estimated remaining time based on current speed |

### Real-time Metrics Panel

Displays key training metrics of the latest step:

| Metric | Description | Ideal Trend |
|------|------|----------|
| Current Loss | Loss value of the model in the current batch | Gradually decreasing |
| Learning Rate | Current learning rate value | Changes according to scheduling strategy |
| Training Speed | Samples processed per second (SPL/s) | Stable |

---

## Metric Charts

### Main Charts (Displayed by Default)

#### Loss Curve

| Attribute | Description |
|------|------|
| **Meaning** | The gap between model prediction and true label |
| **Ideal Trend** | Overall downward trend, eventually leveling off |
| **Abnormal Situation** | Continuous rise or violent oscillation indicates unstable training |

**Analysis Tips**:
- Rapid decline in the early stage is normal.
- Slow decline in the later stage belongs to convergence phenomenon.
- Sudden rise may be due to excessive learning rate.

#### Perplexity Curve

| Attribute | Description |
|------|------|
| **Meaning** | Model's uncertainty in predicting the next token |
| **Calculation** | perplexity = exp(loss) |
| **Ideal Trend** | Decreases synchronously with loss |
| **Reference Value** | The lower the better, usually single digits to tens |

**Analysis Tips**:
- Perplexity is the exponential form of loss, reflecting model quality more intuitively.
- Perplexity of 10 means the model hesitates among 10 candidate words on average.

### Other Charts (Fold Group)

Click **"Other Charts"** fold panel to expand more analysis charts:

#### Gradient Norm

| Attribute | Description |
|------|------|
| **Meaning** | L2 norm of gradient vector, reflecting parameter update magnitude |
| **Ideal Trend** | Relatively stable, no extreme values |
| **Abnormal Situation** | Value explosion indicates gradient explosion, need to reduce learning rate |

**Analysis Tips**:
- Gradient norm too large (e.g., >100) indicates need for gradient clipping.
- Gradient norm too small (close to 0) may be gradient disappearance.

#### Learning Rate Curve

| Attribute | Description |
|------|------|
| **Meaning** | Learning rate value used by current optimizer |
| **Ideal Trend** | Smooth change according to set scheduling strategy |
| **Common Forms** | cosine (cosine decay), linear (linear decay) |

**Analysis Tips**:
- Learning rate rises gradually from 0 during warmup phase.
- Gradually decreases according to strategy during normal training phase.
- Curve shape should be consistent with `lr_scheduler_type` parameter.

#### Evaluation Loss

| Attribute | Description |
|------|------|
| **Meaning** | Loss value calculated on evaluation set |
| **Prerequisite** | Need to set evaluation strategy and evaluation data |
| **Ideal Trend** | Decreases synchronously with training loss, gap not too large |

**Analysis Tips**:
- Training loss decreases but evaluation loss rises = Overfitting.
- Gap too large requires adjustment of regularization parameters.
- Evaluation loss is an important basis for selecting the best checkpoint.

#### Training Speed

| Attribute | Description |
|------|------|
| **Meaning** | Samples processed per second (Samples per Second) |
| **Ideal Trend** | Keep stable |
| **Abnormal Situation** | Violent speed fluctuation may be I/O bottleneck |

**Analysis Tips**:
- Speed is related to batch size and sequence length.
- Speed will drop briefly when saving checkpoints.
- Continuous low speed requires checking data loading configuration.

---

## Chart Data Accumulation Mechanism

> ⚠️ **Important Feature**: Training metrics of the same task will be **accumulated**, not started from scratch.

### Accumulation Principle

When the same training task is trained multiple times (e.g., restart after interruption with modified parameters), the chart will continue from the last step:

**Example Scenario**:
1. Task A first trained to step 30 and interrupted.
2. Continue training after modifying parameters (start from step 0).
3. Chart X-axis displays: `30 → 10 → 20 → 30 → 40 ...`

### Significance of Accumulation

| Scenario | Function |
|------|------|
| Parameter Tuning | Intuitively compare effects of different parameter configurations |
| Resume Training | View the entire training history completely |
| Troubleshooting | Track metric changes before and after training interruption |

### Data Storage

- **Historical Data**: Saved in `training_logs` table of local database.
- **Page Switching**: Chart data will be restored from cache when switching back from other pages.

---

## Training Log

Detailed logs during training are displayed at the bottom of the page:

### Log Types

| Type | Color | Description |
|------|------|------|
| Training Start | 🟢 Green | Training task started |
| Training Step | 🔵 Blue | Recorded once every logging_steps |
| Evaluation Event | 🟡 Yellow | Model evaluation completed |
| Checkpoint Save | 🔵 Blue | Checkpoint saved |
| Training End | 🟢 Green | Training task completed |

### Log Storage

- **Real-time Log**: Displays latest 200 records.
- **Full Log**: Saved in local database, viewable via History Review mode.
- **Persistence**: Training logs are retained after application is closed.

---

## Real-time Monitoring Mode

### Working Principle
1. Push frequency is controlled by `logging_steps` parameter (default push once every 10 steps).
2. Frontend updates charts and metric display after receiving events.

### Data Update Frequency

| Configuration Item | Description |
|--------|------|
| logging_steps | Record log every X steps |
| Backend Throttling | About 300ms sending interval |

### Chart Smoothing

To avoid excessive curve jitter, the system uses a dual smoothing strategy:
- **Exponential Smoothing (EMA)**: Smooth sudden data points.
- **Moving Average (MA)**: Further noise reduction.

---

## History Review Mode

### Entry Method

Select a task with status **Completed** or **Stopped**, system automatically switches to History Review mode.

### Data Loading

1. Load all training logs of the task from database.
2. Parse metric data in logs.
3. Render complete training curves at once.

### Historical Data Limit

- Max display **4000 data points**.
- Excess parts will automatically crop earliest records.
- Ensure chart performance is not affected.

---

## Usage Suggestions

### Monitoring Best Practices

1. **Focus on Loss Curve**
   - Normal Training: Steady decline then leveling off.
   - Need Intervention: Violent oscillation or continuous rise.

2. **Observe Evaluation Loss**
   - Synchronous decline with training loss is ideal state.
   - Consider early stopping when evaluation loss starts to rise.

3. **Check Gradient Norm**
   - Stable value indicates healthy training.
   - Sudden increase requires checking data or reducing learning rate.

### Performance Optimization

| Scenario | Suggestion |
|------|------|
| Long-time Training | Appropriately increase logging_steps to reduce data volume |
| Need Fine Monitoring | Decrease logging_steps to get denser data points |
| Page Lag | Fold infrequently used chart panels |

---

## Next Steps

- [Overview](./overview.md) - Training function overview
- [Prepare Module Details](./prepare.md) - Task creation and parameter configuration
- [Evaluate Module Details](./evaluate.md) - Checkpoint evaluation
- [Test Module Details](./test.md) - Interactive testing
- [Package Module Details](./package.md) - GGUF export
