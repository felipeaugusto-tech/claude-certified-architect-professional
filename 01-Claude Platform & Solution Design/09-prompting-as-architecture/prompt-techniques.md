# 09 · Prompting as Architecture — Prompt Techniques

*Prompt engineering techniques across models · 7 min*

**Once a prompt is designed for reuse, the next question is which technique to use inside it. The technique is chosen by the complexity of the task, not by habit. This screen covers the main techniques, how the choice changes across models, and how to avoid building bias into the prompt.**

## Technique selection by task complexity

| Technique | What it is | When it fits |
|---|---|---|
| **Zero-shot** | Instruction only, no examples. | Well-specified tasks the model already handles reliably; the default to try first. |
| **Few-shot** | A handful of input/output examples in the prompt. | Tasks where the desired format or judgment is easier to show than to describe. |
| **Chain-of-thought** | Prompt the model to reason step by step before answering. | Multi-step reasoning, arithmetic-like logic, or tasks where the path matters to the answer. |

The progression is deliberate: start zero-shot, add examples only if the task needs them, and add explicit reasoning only if the task's structure demands it. Each step adds *tokens* and latency, so reach for the lightest technique that meets the requirement.

> **Tokens** — The unit Claude uses to process text; roughly a word-fragment. Cost and context limits are measured in tokens.

## Behavioral differences across models

The same prompt does not behave identically across model tiers or generations. A more capable model may need less scaffolding (fewer examples, less explicit step-by-step instruction) to reach the same quality, while a less capable model may need more. A prompt tuned for one model is a starting point for another, not a finished artifact. This is why a model swap is treated as a release and gated with an evaluation: the prompt-model pairing is what you are actually shipping.

## Avoiding bias in prompt construction

Prompt construction can introduce bias the task never intended. Leading phrasing, unbalanced examples (few-shot sets that show only one kind of case), and assumptions baked into the instruction all steer the output in ways that can be easy to miss. The discipline is to phrase neutrally, balance examples across the cases the system will actually see, and check whether the prompt presumes an answer it should be eliciting.

## Checkpoint: technique-selection matrix

For each task below, select the technique (zero-shot / few-shot / chain-of-thought) and type a one-sentence reason. Both are required before submitting.

### Task 1

Classify a customer support ticket into one of five standard categories: billing, technical, returns, account, or general.

- Zero-shot
- Few-shot
- Chain-of-thought

<details><summary>Answer</summary>

**Zero-shot** — The task is well-specified, the categories are named, and the model handles classification reliably without examples.

</details>

### Task 2

Extract structured fields, date, vendor, and amount, from expense receipts that vary widely in layout and formatting.

- Zero-shot
- Few-shot
- Chain-of-thought

<details><summary>Answer</summary>

**Few-shot** — The desired extraction format is easier to demonstrate with examples than to describe in instructions, especially given layout variation.

</details>

### Task 3

Determine whether a multi-step contract clause creates a liability under three conditions that interact with each other.

- Zero-shot
- Few-shot
- Chain-of-thought

<details><summary>Answer</summary>

**Chain-of-thought** — The answer depends on a sequence of conditional logic steps; prompting for step-by-step reasoning reduces the chance of skipping an interaction.

</details>

### Task 4

Summarize a 400-word product description into two sentences.

- Zero-shot
- Few-shot
- Chain-of-thought

<details><summary>Answer</summary>

**Zero-shot** — Standard summarization on a well-bounded input; adding examples or explicit reasoning steps adds tokens and latency with no quality gain.

</details>

> **Cost · Complexity · Risk**
>
> **Cost:** Heavier techniques cost tokens and latency on every call. Chain-of-thought on a task that does not need it is a recurring tax for no gain.
>
> **Complexity:** Few-shot examples are content to maintain: as the task evolves, stale examples quietly steer the model wrong.
>
> **Risk:** Bias introduced in the prompt is invisible in any single output and only shows up in aggregate, which is why neutral phrasing and balanced examples are a design requirement, not a polish step.
