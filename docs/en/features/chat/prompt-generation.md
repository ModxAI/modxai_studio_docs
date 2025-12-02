# Prompt Optimization

> v1.0.0
> 2025-12

This article introduces how to use the **Prompt Optimization** feature of ModxAI Studio to help you quickly generate high-quality prompts and improve the effect of conversations with AI.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Prerequisites](#prerequisites)
- [Feature Introduction](#feature-introduction)
- [Usage Steps](#usage-steps)
- [Template Management](#template-management)
- [Variable Replacement Rules](#variable-replacement-rules)
- [Usage Tips](#usage-tips)

---

## Environment Requirements

**No additional environment dependencies are required for this feature.**

The prompt generation feature uses llama.cpp as the inference engine, so there is no need to install environment in **Settings** beforehand.

---

## Prerequisites

Before using the prompt generation feature, the following conditions must be met:

- **Text Model Loaded**: Load a "Chat Model" in the Model Library.
- The magic wand button above the chat page input box will only be enabled after the model is successfully loaded.

When no model is loaded, clicking the magic wand button will prompt "Please enter user content and ensure chat model is loaded".

---

## Feature Introduction

**The core function of intelligent prompt generation is to optimize your prompt expression**, rather than letting the AI complete the task directly.

### What is Prompt Optimization?

When you enter vague or general prompts (such as "help me write a calculator"), it is difficult for AI to understand your real needs. The prompt optimization feature will:

- **Eliminate Vague Expressions**: Convert vague words like "good", "fast", "simple" into specific standards.
- **Supplement Key Information**: Automatically infer and add missing context and constraints.
- **Structured Description**: Organize chaotic ideas into clear, logical instructions.

### Three Optimization Templates

The system provides three optimization strategies, suitable for different input scenarios:

#### 1. General Prompt Optimization (chatprompt_general_optimizer)
- **Applicable Scenario**: Input is relatively clear but expression is not precise enough.
- **Optimization Focus**: Eliminate ambiguity, supplement details, improve operability.
- **Example Input**: "Write an article about AI"
- **Optimized**: "Write a comprehensive article on AI applications for technology practitioners, 1500-2000 words, including practical application cases of current mainstream technology directions (Large Language Models, Computer Vision, Reinforcement Learning), requiring accurate data, specific cases, and professional yet accessible language."

#### 2. Specialized Expert Prompt Optimization (chatprompt_specialized_expert)
- **Applicable Scenario**: Input contains abstract concepts that need to be converted into specific specifications.
- **Optimization Focus**: Convert vague requirements into quantifiable precise descriptions.
- **Example Input**: "Optimize the performance of this code"
- **Optimized**: "Analyze Python code performance bottlenecks, requirements: 1) Use cProfile for performance analysis; 2) Identify functions taking more than 100ms; 3) Propose optimization schemes for nested loops, repeated calculations, and memory copying; 4) Provide performance comparison data before and after optimization (execution time, memory usage); 5) Keep code functionality unchanged."

#### 3. Step-by-Step Requirement Planning (chatprompt_steps_planner)
- **Applicable Scenario**: Complex tasks that need to be broken down into multiple steps.
- **Optimization Focus**: Convert general goals into systematic execution plans.
- **Example Input**: "Write a calculator function in C++"
- **Optimized**: Generate a complete structured prompt containing role definition, execution steps, and delivery standards (including: requirement analysis, architecture design, code implementation, test verification, documentation writing, and other phased tasks).

### Core Features

- **Smart Variable Replacement**: Automatically fill your input content into template placeholders.
- **Template Combination**: System templates and user templates are paired for use, and can also be manually combined.
- **One-Click Application**: Optimized prompts can be directly filled into the chat input box.
- **Template Management**: Save and delete custom templates.

---

## Usage Steps

### 1. Open Prompt Generation Dialog

Click the **Magic Wand Icon** button above the input box at the bottom of the chat page.

### 2. Select Optimization Template

The dialog is divided into two template selection areas:

#### System Prompt Template (Define Optimization Expert Role)
- **Function**: Set the capabilities and optimization principles of the prompt optimization expert.
- **Location**: Left dropdown box.
- **Options**:
  - **General Prompt Optimization**: Basic optimization expert for eliminating ambiguity and supplementing context.
  - **Specialized Prompt Optimization**: Engineering expert for converting abstract requirements into precise specifications.
  - **Step-by-Step Requirement Planning**: Strategic planning expert for breaking down complex tasks.

#### User Prompt Template (Define Optimization Instruction)
- **Function**: Clarify requirements and output format for the AI optimization expert.
- **Location**: Right dropdown box.
- **Variable Support**: Use placeholder `{{userPrompt}}` in the template, which will be replaced by your original input.

**Template Pairing Mechanism**:
- **Auto Pairing**: When selecting a system template, the matching user template will be automatically selected.
- **Manual Combination**: You can also manually select different combinations of system and user templates (although default pairing works best).

### 3. Edit Prompt Content

After selecting a template, the content will be automatically filled into the text box below:

- **System Prompt**: Can be edited directly to set the AI role.
- **User Prompt**: Can be edited directly to define task requirements.
  - Supports placeholders: `{userPrompt}` or `{{userPrompt}}`.
  - Will be automatically replaced by "User Input Content" below when generating.

### 4. Enter Original Prompt to be Optimized

Enter your original prompt (usually vague, general description) in the **"User Input Content"** text box.

**Correct Examples**:
```
Write a calculator
```
```
Analyze the problems in this code
```
```
Summarize machine learning
```

**Incorrect Examples** (Already clear task instructions, no need to optimize):
```
Please translate "Artificial Intelligence is changing the world" into English
```
```
Calculate the result of 123 + 456
```

> **Important Note**: If your input is already very clear and specific, using prompt optimization may result in empty or meaningless results. This feature is designed for vague and general expressions.

### 5. Generate Optimized Prompt

Click the **"Generate"** button, and the system will:
1. Replace your original prompt into the `{{userPrompt}}` placeholder of the user prompt template.
2. Combine the system prompt (defining expert role) and user prompt (defining optimization task).
3. Call the **currently loaded AI model** to analyze your input, supplement details, eliminate ambiguity, and structure the expression.
4. The optimization result is displayed in the **"Generation Result"** text box.

**Generation Process Example**:
- **Original Input**: "Write a sorting algorithm"
- **Template Used**: Specialized Prompt Optimization
- **Optimization Result**: "Implement Quick Sort algorithm (Python), requirements: 1) Use recursive implementation; 2) Handle boundary cases of empty arrays and single-element arrays; 3) Support custom comparison functions; 4) Time complexity O(nlogn), space complexity O(logn); 5) Add detailed Chinese comments explaining partition logic and recursion termination conditions; 6) Provide 3 test cases (integer array, string array, custom object array)."

### 6. Apply to Chat

- **Apply to Chat** button: Directly fill the generation result into the chat input box.
- **Cancel** button: Close the dialog without saving any content.

---

## Template Management

### Save Template

When you have edited a satisfactory prompt, you can save it as a template for next use:

1. Click the **"Save Template"** button.
2. Enter template information:
   - **Template Name** (Required): Name for easy identification.
   - **Template Description** (Optional): Explain the purpose of the template.
3. Click Confirm.

**Save Rules**:
- Will save both system prompt and user prompt as a pair of templates.
- System prompt template name will automatically add a suffix.
- User prompt template name will automatically add a suffix.

### Delete Template

Delete unwanted templates:

1. Select the template to delete (system or user).
2. Click the **Delete Button** (Trash can icon).
3. Confirm the delete operation.

**Note**:
- Deleting a system template will not automatically delete the paired user template.
- Deleting a user template will not automatically delete the paired system template.
- Delete operation cannot be undone.

---

## Variable Replacement Rules

User prompt templates support the following placeholders, which will be automatically replaced by "User Input Content" when generating:

### Supported Placeholder Formats (Must keep one of them)

**Curly Brace Format**:
- `{userPrompt}`
- `{content}`
- `{user_content}`
- `{query}`
- `{originalPrompt}`
- `{input}`
- `{text}`

**Mustache Format**:
- `{{userPrompt}}`
- `{{content}}`
- `{{user_content}}`
- Etc. (Corresponding to curly brace format)

### Variable Replacement Example

Here is how to use placeholders in user prompt templates:

**User Prompt Template Content** (General Optimization):
```
Refine the following user prompt by removing ambiguity and enriching essential context.

Critical Instructions:
- Your objective is to optimize the prompt itself, not to execute or respond to its content.
- Output only the enhanced prompt—no explanations, preambles, or meta-commentary.
- Preserve the user's original intent while improving precision and completeness.

User prompt requiring optimization:
{{userPrompt}}

Enhanced prompt:
```

**User Input Original Prompt**:
```
Help me analyze this dataset
```

**Complete Instruction Received by AI** (After replacement):
```
Refine the following user prompt by removing ambiguity and enriching essential context.

Critical Instructions:
- Your objective is to optimize the prompt itself, not to execute or respond to its content.
- Output only the enhanced prompt—no explanations, preambles, or meta-commentary.
- Preserve the user's original intent while improving precision and completeness.

User prompt requiring optimization:
Help me analyze this dataset

Enhanced prompt:
```

**Optimization Result Returned by AI** (Example):
```
Perform Exploratory Data Analysis (EDA) on the dataset, requirements: 1) Load CSV format dataset; 2) Output basic dataset information (number of rows and columns, field types, missing value statistics); 3) Generate descriptive statistics for numerical fields (mean, median, standard deviation, min/max); 4) Plot distribution charts for key fields (histogram, box plot); 5) Analyze correlation between fields and generate correlation heatmap; 6) Identify outliers and data quality issues; 7) Output analysis report in Markdown format.
```

---

## Usage Tips

### 1. Choose Template Based on Input Characteristics

**Judgment Criteria**:

| Input Feature | Recommended Template | Reason |
| :--- | :--- | :--- |
| Vague expression, general vocabulary ("Help me write a XX") | General Prompt Optimization | Supplement details, eliminate ambiguity. |
| Contains abstract concepts ("High performance", "User friendly") | Specialized Expert Prompt Optimization | Convert to quantifiable specific indicators. |
| Complex task, multiple steps ("Develop a complete XX system") | Step-by-Step Requirement Planning | Break down into systematic execution plan. |
| Already clear and specific instruction | **No Optimization Needed** | Use original prompt directly. |

**Example Comparison**:

```
❌ Not Suitable: Already clear instruction
Input: "Translate 'hello world' into Chinese"
Result: Optimization makes it redundant or meaningless.

✅ Suitable for General Optimization: Vague expression
Input: "Translate this paragraph"
Result: Supplement language pair, quality requirements, format requirements, etc.

✅ Suitable for Specialized Optimization: Abstract concepts
Input: "Optimize code performance"
Result: Clarify performance indicators, analysis methods, optimization dimensions.

✅ Suitable for Step Planning: Complex task
Input: "Write a calculator function in C++"
Result: Break down into requirement analysis, architecture design, code implementation, test verification, etc.
```

### 2. Understand Template Pairing Logic

- **Default Pairing**: System template and user template use the same optimization strategy (general, specialized, steps).
- **Manual Combination**: Technically can be mixed (e.g., system uses general, user uses specialized), but default pairing usually works best.
- **Why Separate**: System prompt defines "optimization expert's capability", user prompt defines "specific optimization task", separation makes templates more flexible.

### 3. Edit Template Content

After selecting a template, you can modify the content of the system or user prompt:

- **System Prompt**: Adjust expert capability description, optimization principles.
- **User Prompt**: Modify instructions to AI, output format requirements.
- **Note to Keep**: `{{userPrompt}}` placeholder must be kept, otherwise your input cannot be filled.

### 4. Iterative Optimization Strategy

If the generation result is not ideal:

1. **Check Input**: Confirm if the original prompt is vague enough (too specific instructions do not need optimization).
2. **Switch Template**: Try other optimization strategies.
3. **Adjust Instruction**: Modify requirements in the user prompt template (e.g., "increase technical depth", "simplify expression", etc.).
4. **Regenerate**: Click "Generate" button to optimize again.

### 5. Save Custom Templates

Scenarios suitable for saving as templates:

- **Specific Domain Optimization**: Such as "Frontend Code Optimization", "Academic Paper Prompt Optimization".
- **Fixed Output Format**: Such as "Generate Step-by-Step Plan in Markdown Format".
- **Enterprise/Team Standard**: Unified team prompt optimization standard.

**Naming Suggestions**:
- Describe optimization focus: e.g., "Security-Focused Code Optimization".
- Mark applicable scenario: e.g., "Data Analysis Task_Step Planning".
- Distinguish versions: e.g., "General Optimization_v2 (Simplified)".

---

## FAQ

### Generate button cannot be clicked?

**Reason**:
- Chat model not loaded.
- "User Input Content" is empty.

**Solution**:
1. Go to Model Library to load "Chat Model" (Text Model).
2. Enter your original prompt in the "User Input Content" box.

---

### Generated result is empty or meaningless?

**Most Common Reason**: Your input is already too specific, no room for optimization.

**Example**:
```
❌ Incorrect Use:
Input: "Please tell me the result of 123+456"
Problem: Instruction is already very clear, AI cannot "optimize" this prompt.

✅ Correct Use:
Input: "Write a calculator"
Optimized: "Develop a command-line calculator program based on Python, requirements: 1) Support four arithmetic operations and bracket priority; 2) Handle decimals and negative numbers; 3) Detect division by zero error and prompt; 4) Support continuous calculation; 5) Provide clear user input prompts and error handling; 6) Add code comments."
```

**Solution**:
- Use more vague, general input.
- If the task itself is very clear, enter it directly in the chat box, no need for prompt optimization.

---

### Which template should I choose?

**Simple Judgment**:

1. **Input is simple and general** (e.g., "Write a crawler") → **General Optimization**
2. **Input has abstract vocabulary** (e.g., "High performance", "User friendly") → **Specialized Optimization**
3. **Input is a complex task** (e.g., "Develop a blog system") → **Step Planning**

**When Unsure**: Try General Optimization first, switch to other templates if not satisfied.

---

### Variable not replaced?

**Check Items**:
- Does the user prompt template contain `{{userPrompt}}` or other supported placeholders?
- Is the placeholder format correct (Curly braces `{}` or double curly braces `{{}}`)?
- Did you enter content in the "User Input Content" box?

**Tip**: System default templates already contain placeholders, usually this problem does not occur. Only need to pay attention when customizing templates.

---

### Why separate System Prompt and User Prompt?

- **System Prompt**: Defines the identity, capability, and optimization principles of the "Prompt Optimization Expert" (similar to role setting).
- **User Prompt**: Defines the specific optimization task and output requirements (similar to task instructions).

**Analogy**:
- System Prompt = Hiring an "Expert proficient in prompt optimization".
- User Prompt = Giving this expert the task "Optimize this prompt for me".

Separation design makes templates more flexible, allowing combination of different expert capabilities and task requirements.

---

### Can the optimized result be optimized again?

Yes, but usually not recommended:

1. **First Optimization**: Optimize vague prompt into clear instruction.
2. **Second Optimization**: Since it is already very clear, usually no valuable result is obtained.

**Better Practice**:
- Directly modify the first optimization result (manually adjust details).
- Or adjust the original input and regenerate.

---

After completing this chapter, you can continue to learn about:
- **AI Chat**: How to use generated prompts for conversation.
- **Inference Parameter Settings**: Advanced parameters for adjusting generation effects.
- **Data Collection**: Save excellent conversation records.
