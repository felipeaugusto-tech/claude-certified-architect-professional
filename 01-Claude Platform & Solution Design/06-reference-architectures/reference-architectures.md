# 06 · Reference Architectures — Reference Architectures

*The shapes the industry has already paid to learn · 15 min*

**Choosing a pattern gets you the right structure. Patterns give you the shape. The next question to ask is how that structure connects to everything around it. Reference architectures will give you the wiring.**

## Reference architectures: what good looks like and where projects go wrong

Reference architectures are references, not blueprints to adhere to. The goal is not matching a problem to a fixed design and implementing it as drawn. Every partner workload is unique, so your goal here is to understand these common patterns well enough to generalize from them. Ultimately, you should be able to take the shape that fits, adapt it to the workload in front of you, and recognize when a workload draws on more than one pattern at once. Most partner problems map to one of a handful of reference architectures already proven in the Claude ecosystem: documented patterns for how to wire an LLM application together to solve a recurring class of problem. The table below covers common reference architectures, what they look like when they're built well, and the failure modes that show up repeatedly.

### Agent (see S11)

**What good looks like:** The model works toward a goal by deciding which tools to call and in what order. Autonomy is kept in check by limiting what the tools can do and setting a budget on how many turns the model gets. Use this when the path through the work can't be written in advance: investigating a codebase, pulling from multiple research sources, or triaging complex customer cases.

**Where projects go wrong:** Unbounded autonomy: Giving the model tools that change state with no human review, no turn limit, and no way to measure whether the goal was met.

### Retrieval-augmented generation (RAG) (see S11)

**What good looks like:** A stable knowledge corpus, such as product manuals, internal docs, or regulatory text, is chunked and indexed. When a question comes in, the most relevant chunks are retrieved and passed to the model as context.

**Where projects go wrong:** Using RAG to answer questions about live state: order status, inventory levels, ticket queues. The index is a snapshot. If the underlying data has changed since the last refresh, the answer will be wrong.

### Document processing pipeline → Evaluator-optimizer (see S11)

**What good looks like:** Structured extraction from semi-structured documents like claims, invoices, and contracts. The pipeline handles OCR, extracts fields against a schema, validates the output, and routes exceptions. An evaluator-optimizer is common here because first-pass extraction on edge cases isn't reliable enough to trust without a check.

**Where projects go wrong:** No exception path. Low-confidence extractions go through the same pipeline as clean documents, with no human gate to catch the ones the model got wrong.

### Customer-service / ticket triage → Routing (see S11)

**What good looks like:** Classify intent and what the user is asking for, then route to the right backend: a knowledge retrieval layer for documentation questions, a transactional API for live state queries like order status or account changes, and a human approval layer for high-consequence actions.

**Where projects go wrong:** Using retrieval for live order status instead of calling the API directly. No escalation path to a human. Deploying an agent variant before the simpler routed workflow has been properly measured.

### Coding agent (agentic exploration with deterministic edit/test/review steps) (see S11)

**What good looks like:** The work splits into two phases. First, the agent investigates the codebase to understand what needs to change: this part is agentic because the path through an unfamiliar codebase can't be written in advance. Second, the actual edits follow deterministic steps: parse, plan, propose, test, review. Subagents handle isolated tasks with enough context to do the job but not so much that the steps become unmanageable.

**Where projects go wrong:** Letting the agent edit and commit without a human review gate. Not tracking regression rates against an eval set for each language or framework in the codebase. Treating the whole thing as a conversation rather than a structured pipeline with defined handoffs.

Full *RAG* implementation depth, including chunking strategies, embedding approaches, hybrid lexical-plus-semantic retrieval, and reciprocal rank fusion, is covered in the RAG pipeline design screen that follows.

> **RAG** — A pattern where Claude retrieves relevant external content at request time and uses it to ground its response.

## How to decide if a problem needs one or several patterns

Real partner problems frequently sit at the boundary between two architectures. A routing workflow might hand certain intents to an agentic investigation loop. A document processing pipeline might use RAG over policy text when it hits an exception case. Drawing on more than one pattern is sometimes the right answer.

What matters is why you are reaching for a second pattern. Don't think of patterns as pieces you snap together. Look at why each one works and shape the idea to fit your problem. Draw on a second pattern when the two parts of your problem break in different ways that are worth managing separately. If you are reaching for a second pattern because you haven't decided what problem you're solving, adapt a single pattern instead. That's a design decision you're deferring, not a pattern you're applying.

## The most common mistake: retrieval applied to live state

The most common reference architecture mistake is using retrieval where a tool call belongs. You can recognize it by looking for these symptoms: stale chunks, results that shift with each index refresh, answers that contradict what's in the database. A better embedding model or a shorter refresh interval won't fix this issue. Instead, call the system that owns the live state directly rather than retrieving a cached version of it.

> **Cost · Complexity · Risk**
>
> **Cost:** Composing two reference architectures roughly double the surface area you must maintain. When in doubt, pick one.
>
> **Complexity:** Each reference architecture carries its own eval contract. You need separate eval sets per architecture, not a single eval set for the composed system. A system that looks healthy at the top level can be masking failures in one of its components.
>
> **Risk:** Misapplying retrieval to live state produces stale but confident answers. The system looks healthy from the outside: normal latency, no errors. Detection cost is high because there's no signal that something is wrong until a user notices the answer doesn't match reality.
