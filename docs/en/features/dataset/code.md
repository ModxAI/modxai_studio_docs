# Code Analysis (CODE)

Analyze a local code repository and save the results for reuse. Review which files parsed successfully, then export data for a knowledge base, pretraining, supervised fine-tuning (SFT), or further processing.

## Analyze a repository

1. In **Settings → System → Environment Management**, install **Code Analysis**. If the app still reports that parsing resources are unavailable, follow the prompt to install **All(Recommend)**. To generate SFT question-and-answer pairs, also prepare a local chat model in the Model Center.
2. Go to **Self Improvement → Dataset → CODE → Data source**. Choose the source folder and where to save analysis results. Use separate folders for source and output.
3. To skip build directories or generated files, add one rule per line under **Exclude Files or Folders**, such as `build/` or `*.min.js`.
4. Select **Start Analysis**. When it finishes, open **Review analysis** and check the parsed count, languages, and file statuses.

## Review and reuse results

- Filter by file name, language, or status. Double-click a file to see its source, parsing diagnostics, or available dataset generation results.
- **Excluded** means the file matched an exclusion rule. **Unsupported** and **Syntax errors** do not indicate successful parsing. Check important directories before exporting.
- To reuse results, select the original output folder under **Data source**, choose **Open Results**, and select a **Revision**. When the source changes, analyze it again with the same source and result folders. If the contents are unchanged, no new revision is created.

## Export a dataset

In **Export dataset**, choose an output type and destination:

| Type | Use |
| --- | --- |
| **Knowledge base** | Export code chunks for retrieval. Select **Use in knowledge base (RAG)** to configure and build the knowledge base. |
| **Pretraining** | Export analyzed code as data for pretraining preparation. |
| **SFT** | Choose a local chat model to generate question-and-answer pairs. Add **Additional Instructions** if needed. After generating and exporting, review sample entries in the file details before using them for training. |
| **Raw data** | Export source code and analysis results for further processing. |

Exporting does not start training or build a vector index. In the next screen, review the sample file or folder, model, and settings before starting. For private repositories, make sure you have permission to use the code and exclude credentials or other files that should not enter the dataset.

Related: [RAG Knowledge Base](./rag.md) · [Training Preparation](../training/prepare.md) · [SFT Text Data Processing](./sft.md)
