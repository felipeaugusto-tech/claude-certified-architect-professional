# 08 · Model & Context Strategy — Model & Context Strategy

*Model, context window, and context strategy · 13 min*

**By this point you should have a pattern and a reference architecture, but not a shippable system. Three decisions remain, and each one determines what the same architecture costs at scale: 1. Which model fits the task, 2. How much of the context window to actually use, and 3. Whether your context strategy should be progressive or monolithic. These decisions compound across every request at production volume.**

## Distinct terms that are easy to conflate

The following terms often get used interchangeably in practice but conflating them produces reference architecture mistakes. Take time to carefully review each term in detail:

- **Context window.** The model's active attention space. Everything inside the context window is available for reasoning and everything outside it doesn't exist to the model. The context window resets between calls unless your application explicitly manages continuity.
- **Retrieval.** Fetched external knowledge, pulled at query time from a corpus the model doesn't hold in memory. Retrieval augments the context window; it doesn't replace it. The model only sees what the retriever surfaces.
- **Persistent application state.** Owned and managed by your system, not the model. Order status, user records, account balances. The model does not have inherent access and requires a tool call to get it.
- **Summaries and memory layers.** Application-managed continuity across turns or sessions. The model has no native memory between calls, so anything that persists does so because your application stored it and passed it back in. This is an architectural choice, not a model capability.

## Model selection: start with Sonnet, move deliberately

The Claude model family currently consists of Opus, Sonnet, and Haiku, each optimized for different cost, latency, and capability tradeoffs.

Opus is Anthropic's most capable model available for use, suited for demanding reasoning, advanced coding, and research synthesis where Sonnet doesn't meet your quality bar. The default starting point is still Sonnet. Move up to Opus only when an eval set tells you Sonnet isn't meeting your quality bar. Move down to Haiku only when an eval set confirms the quality tradeoff is acceptable for your specific task. Your decision to move models should always be measured, not reflexive.

## Context-window sizing: the working-memory cliff

Everything the model attends to lives in the context window. Inside the window, attention is available. Outside it, the model has no access at all. Working memory is the property with the hardest edge of the four: things work until they don't, and then the transition is abrupt.

The context window is measured in tokens. How much text one token covers varies by model generation, tokenizer, and language, so treat any fixed characters-per-token ratio as a rough illustration rather than a rule. Measure instead of estimating: every API response reports actual token counts in its `usage` field, and those measured counts are what the context limit and billing apply to. Everything that enters the context window, your system prompt, the conversation history, retrieved documents, tool outputs, and the model's responses, is counted in tokens. This matters for two reasons: the context window has a fixed token limit, and you're billed per token on every API call. Both constraints show up directly in the decisions covered in this section.

The practical implication is direct: do not budget the full window. Budget for the largest realistic conversation, plus retrieved context, plus system prompt, plus working scratch, plus margin for growth. The context window is a ceiling, not a target, so designing towards the ceiling means you hit it in production.

## Context strategy: spectrum between progressive and monolithic

Every production workload makes a choice, implicitly or explicitly, about how context reaches the model on each call. The choice sits on a spectrum between two poles.

At one end, a monolithic context strategy places everything into the prompt at once: the full document, the full conversation history, the full retrieved corpus. It works for bounded tasks with predictable input sizes. It's also the strategy that hits the working-memory cliff in production, because context accumulates across turns and the window fills silently before anyone notices something has been truncated.

At the other end, a progressive context strategy stages the context instead: it retrieves just-in-time, summarizes across turns, and loads only what the next step needs. Most production workloads belong here.

Two other patterns sit between the poles and show up often enough to treat as strategies. The table below lays out where each of the four strategies earns their place and where they break down.

### Monolithic, load the full required context into a single prompt

**Where it earns its place:**

- Bounded tasks with predictable input size.
- Stable prefixes that benefit from prompt caching.
- Single-shot Q&A where retrieval latency isn't worth paying.
- Reasoning that genuinely requires simultaneous access to all material.

**Where it breaks down:** Conversations or tool loops where context accumulates turn over turn. Cost and latency scale linearly with input length. Attention quality can degrade on very long contexts well before the hard limit is reached.

### Progressive, carry forward only what the next step needs

**Where it earns its place:**

- Multi-turn dialogue and iterative refinement.
- Agent loops where each step depends mainly on recent state.
- Workflows that decompose into stages with narrow, well-defined handoffs.

The right default for most production workloads.

**Where it breaks down:**

- Tasks requiring long-range coherence across the full history.
- Decisions that depend on detail dropped in an earlier turn.
- Prompt caching is harder when carried-forward context mutates each turn.

The exact input the model saw at step N is no longer reconstructable, which complicates debugging.

### Retrieval (RAG), fetch relevant chunks from an external store at query time

**Where it earns its place:**

- Knowledge bases too large to fit in context.
- Sources that change faster than the prompt is redeployed.
- Domains where any single query needs only a small slice of available material.
- Cases where source citation is a requirement.

**Where it breaks down:**

- Queries requiring synthesis across many documents the retriever scores independently.
- Chunking that splits semantic units like tables, code blocks, or multi-paragraph arguments.
- Recall failures where the correct document never enters the top-k.
- Retrieval quality becomes a system you must evaluate and maintain.

### Compaction, periodically summarize or compress accumulated context

**Where it earns its place:**

- Long-running agents and conversations where the full transcript is wasteful but recent state matters.
- Phase transitions in multi-step workflows that can checkpoint to a clean summary.
- Sessions that would otherwise hit context limits mid-task.

**Where it breaks down:**

- Summaries that drop load-bearing detail: exact identifiers, numeric values, prior decisions, edge cases mentioned once.
- The summarizer is itself a model call with latency, cost, and failure modes.
- Compaction is largely one-way.
- Measuring summary fidelity against the original transcript is an unsolved evaluation problem.

## In practice, strategies may be combined

The four strategies above are presented separately for learning clarity, but production systems almost always combine them. Each handles a different dimension of the context problem, so a well-designed system layers them deliberately rather than picking one. A worked example, long-running coding agent:

| Phase | What's happening | Strategy in play |
|---|---|---|
| Session start | Load the task description and the few files the user explicitly referenced | **Monolithic prefix**, A small, stable context loaded once, ideal for prompt caching. |
| Active work | Each tool call (read file, run tests, edit) appends to the working context | **Progressive recent state**, The latest additions is what the next step needs. |
| Discovery | Agent realizes it needs a file it didn't load initially; searches the codebase and pulls in matches | **Just-in-time retrieval**, The corpus is too large to preload, and only a relevant slice is fetched on demand. |
| Context filling | After many turns, early exploration is taking up space; the conclusions matter but the verbatim tool outputs don't | **Compaction**, Summarizes "what we tried and what we learned," preserving only the insights and decisions that carry the work forward. |

Notice that no single strategy could carry this workload. Monolithic alone hits the context limit. Progressive alone has no way to surface code the agent didn't initially load. Retrieval alone loses the thread of what's been tried. Compaction alone has nothing to compact until other strategies have built up the trajectory.

The architectural takeaway: when designing a context system, ask yourself these separate questions:

- What does the model need at the start? → drives the monolithic baseline
- What does it need from the most recent steps? → drives the progressive window
- What might it need to fetch on demand? → drives the retrieval layer
- What earlier material can be compressed without losing decision-relevant detail? → drives the compaction policy

Context strategy and context sizing are separate decisions that interact but don't determine each other. Treating them as one is where most context-management designs go wrong.

## What extended thinking controls

Extended thinking is a per-request capability: the model works through the problem in a separate block of thinking tokens before it produces the final answer. How you control it has changed across model generations. On Claude Opus 4.6 and later, Claude Sonnet 4.6 and later, and Claude Sonnet 5, adaptive thinking with the effort parameter is the recommended control: you set how much reasoning effort to apply rather than configuring a token budget. Adaptive thinking is the only thinking mode on Claude Fable 5. The older manual thinking-token budget (`budget_tokens`) is deprecated on the 4.6 generation and removed on Claude Sonnet 5, where it returns a 400 error; verify current model support against platform.claude.com at publish time. Thinking tokens are billed as output tokens at the model's standard output rate and generating them adds latency to the call. When extended thinking is not engaged, none of those tokens are generated and none are billed. The decision to use extended thinking anchors on whether to add a billed reasoning pass to a call. The model reasons internally either way. What you're choosing is whether to spend tokens and latency on an expanded pass. On models that support extended thinking, the API may return a summarized representation of the thinking process rather than the full reasoning output. You are billed for the thinking tokens actually consumed during reasoning, not the length of the visible summary.

Once that distinction is clear, the decision rule is straightforward. Extended thinking is a cost and latency tradeoff. Run your evals without it first: if accuracy still isn't meeting your requirements after you've worked on the prompt itself, then consider enabling it. With extended thinking engaged, you pay for the thinking tokens and the additional latency on every call. The case for turning it on should come from a measured accuracy gap, not the assumption that it's a mode that "it can't hurt." Without that evidence, you're paying the cost with no proof it's moving your accuracy metrics.

## Gate every model change with an eval before you ship it

Any change to the model is a change to the system's behavior. A swap between two models is a code deployment and should be treated as such. At a minimum you need three things:

- A curated test set of prompts with known-good outputs that covers the real distribution of work the system sees
- A grading function (model-graded against a rubric, or programmatic where you can express the check in code)
- A delta threshold set in advance below which you do not ship. Set the threshold before you run the eval. If you set it after, you are not setting a standard, you are writing the acceptance criteria after the build.

> **Worked case: a Sonnet to Haiku downgrade, done well**
>
> The case below shows the full sequence of a model downgrade run against a real production constraint. The pattern to carry into your own work is the rollback criterion: set in advance, refused to negotiate when the data came in, and used to motivate a partial migration instead of a full one.
>
> A document-intelligence pipeline has been running on Sonnet for six months and has used their entire budget. Now the team wants to move to Haiku.
>
> - The team builds an eval set of 250 representative documents with hand-validated extraction targets, stratified across the document types that show up in production traffic. The sample is sized so that per-document-type scores remain meaningful, not just the overall average.
> - The team runs both models against the same set and scores each extraction with the same grading rubric.
> - The regression signature comes back as follows: Sonnet scores 0.94 on average, Haiku scores 0.86 on average, and the variance is concentrated in two document types where Haiku scores 0.71 and 0.74 respectively. The rest of the document types come in within tolerance.
> - The rollback criterion was set in advance: if any single document type drops below 0.85, the migration is rejected. Two document types crossed that line, so the migration as proposed is rejected.
> - The salvage move is to route those two document types to Sonnet via the existing classifier, and route the other types to Haiku. Cost drops materially without taking the regression on the difficult document types.
>
> Rather than the specific scores, your takeaway from this case should be focused on how the rollback criterion was decided before the data came in. This meant when the data came in, the team did not have to negotiate with itself, and the eval set surfaced a partial-migration option that a single overall score would have hidden.

> **Forward pointer**
>
> Model selection and context strategy are two of the prompting-area levers. You will learn about two other prompting area levers (system-prompt design and prompt reuse) later in this module.

> **Cost · Complexity · Risk**
>
> **Cost:** Monolithic context is the silent budget killer. In a document-heavy pipeline, a conversation that starts with a 4,000-token prompt can be carrying 80,000 tokens by turn 30. The per-call cost grows with the conversation, and there's no visible signal until it shows up in billing.
>
> **Complexity:** A model swap looks like a one-line configuration change, but it rewrites how the whole product behaves. Treat model swaps as releases.
>
> **Risk:** No eval set means no rollback signal. A regression discovered in production is a regression that wasn't detected at all, because by the time you see it, the user already has.
