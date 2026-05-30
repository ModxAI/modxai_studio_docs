# Advanced Settings

> v1.1.0
> 2026-05

Advanced settings allow fine-tuning the agent's behavior strategies. All parameters have reasonable defaults and generally do not need modification. Advanced users can adjust them as needed.

## Table of Contents

- [Parameter Groups](#parameter-groups)
- [Tool Global](#tool-global)
- [File Reading](#file-reading)
- [Web Search](#web-search)
- [Browser](#browser)
- [Crawler](#crawler)
- [Content Cache](#content-cache)
- [Online Model](#online-model)
- [Context Compression](#context-compression)
- [Evaluate Node](#evaluate-node)
- [Security](#security)

---

## Parameter Groups

Advanced parameters are grouped into 10 categories, displayed as collapsible panels in the interface:

| Group | Description |
|-------|-------------|
| Security | Security review toggles and command blacklist |
| Tool Global | Global limits affecting all tools |
| File Reading | File read/write restrictions |
| Web Search | Search engine parameters |
| Browser | Browser operation parameters |
| Crawler | Web crawling parameters |
| Content Cache | Context content caching strategy |
| Online Model | Online API calling strategy |
| Context Compression | Conversation compression threshold control |
| Evaluate Node | Answer evaluation retry strategy |

---

## Tool Global

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Tool Execution Timeout | 600 s | 0.1 ~ 3600 s | Maximum execution time for a single tool call. The tool call is interrupted upon timeout. |
| Page Content Limit | 5000 | 1000 ~ 100000 chars | Maximum character count when extracting content from web pages, browsers, and crawlers. |
| Default Page Lines | 100 | 50 ~ 5000 lines | Default lines per page when reading files. |
| Repeated Output Threshold | 20 | 1 ~ 200 | How many consecutive identical tool outputs before protective mechanism is triggered. |
| Repeated Call Threshold | 50 | 1 ~ 100 | Maximum consecutive identical tool calls before execution is paused. |

---

## File Reading

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| File Read Limit | 10MB | 1 MB ~ 100 MB | Maximum bytes allowed for reading a single file, displayed in MB. |

---

## Web Search

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Search Thread Timeout | 120 | 1 ~ 120 s | Thread-level timeout for search tasks. |
| Request Timeout | 60 | 1 ~ 300 s | Timeout for a single HTTP request. |
| Max Search Results | 10 | 1 ~ 50 | Maximum number of result entries returned per search. |
| Content Fetch Limit | 10 | 1 ~ 10 | Maximum number of results from which content is automatically fetched after search. |

---

## Browser

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Default Timeout | 30 | 1 ~ 300 s | Default timeout for browser-related operations. |
| Action Timeout | 10 | 0.1 ~ 120 s | Timeout for a single browser interaction (click, input, etc.). |

---

## Crawler

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Max Crawl Pages | 100 | 1 ~ 200 | Maximum number of pages per crawl task. |
| Max Crawl Depth | 3 | 0 ~ 10 | Maximum recursive depth for link tracking. |
| Fetch Timeout | 30 | 1 ~ 300 s | Timeout for fetching a single page. |
| Max Links Per Page | 24 | 1 ~ 200 | Maximum number of links to track per page. |

---

## Content Cache

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Cache Threshold Ratio | 0.15 | 0.01 ~ 0.95 | Data enters cache when its proportion of context exceeds this value. |
| Query Return Count | 10 | 1 ~ 50 | Maximum entries returned per cache query. |
| Query Content Limit | 5000 | 500 ~ 50000 chars | Maximum character count for cache query results. |
| Cache Total Limit | 130000 | 10000 ~ 20000000 chars | Maximum total characters for content cache. |

---

## Online Model

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Max Retries | 3 | 0 ~ 5 | Maximum retries after an online API request failure. |
| Max Retry Wait | 60 | 1 ~ 600 s | Maximum wait time between retries. |
| Rate Limit Wait | 60 | 1 ~ 120 s | Wait time after rate limiting is triggered. |
| Prompt Caching | On | On/Off | Whether to enable prompt caching for online models. |
| OpenRouter Cache TTL | 600 | 10 ~ 86400 s | Cache validity period for the OpenRouter model list. |
| OpenAI Cache Retention | 24h | String | Retention strategy for OpenAI prompt cache. |
| Claude Cache TTL | 1h | String | Validity period for Claude prompt cache. |

---

## Context Compression

Context compression parameters control when conversation history triggers compression, and the proportional allocation of content at each priority level after compression.

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| P0 Ratio | 0.2 | 0 ~ 1 | Budget ratio for core system prompts and current task. |
| P1 Ratio | 0.2 | 0 ~ 1 | Budget ratio for tool results and task status. |
| P2 Ratio | 0.4 | 0 ~ 1 | Budget ratio for conversation history. |
| P3 Ratio | 0.2 | 0 ~ 1 | Budget ratio for summaries, knowledge indices, and memory. |
| Compression Trigger Ratio | 0.8 | 0.1 ~ 0.9 | Model input proportion of the context window at compression time. |

---

## Evaluate Node

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| Evaluate Retry Limit | 2 | 0 ~ 8 | Maximum retries when the evaluate role is unsatisfied with the response. Exceeding this will output the current result directly. |

---

## Security

### Command Blacklist

- **Default**: Preset list of common dangerous commands.
- **Description**: List of terminal commands prohibited from execution. Supports exact match, command prefix match, and wildcards `*` `?`. Base commands automatically cover their argument variants (e.g., `rm` matches `rm file.txt`), and can also recognize real subcommands in chained commands, pipes, and cmd/powershell/bash wrapper commands.
- **Format**: Separate multiple commands with commas or newlines.

The default blacklist includes `rm`, `del`, `format`, `shutdown`, `taskkill`, and other common dangerous commands. You can add or remove entries as needed.

---

## Modifying Parameters

- All advanced parameter changes take effect after clicking the **Apply** button.
- Values outside the allowed range are clamped to the min/max interval.
- Full parameter configurations can be saved and restored via **Preset Management**.
