# 02 · How Claude Behaves — How Claude Behaves

*The four properties architects design around · 12 min*

**Before you decide what Claude should do in a solution, you need a clear understanding of how Claude behaves. Four properties of the model shape every following design decision. None of these properties are a flaw to be fixed, each is a force you design around – like how a structural engineer designs around the properties of their building materials.**

Consider this screen as initial groundwork, you are not being asked to make any design decisions at this point. The goal is to recognize the four properties by name and understand each property's design consequences to prepare you to make informed choices in judgment exercises later in the module.

## The four properties and their design consequences

For each property below, the same characteristics that make Claude capable in one situation are the same that makes it fail in another. Read each row as a capability paired with its matching limitation, and the mitigation that an architect reaches for.

## Next-token prediction

**Capability:** Tasks built on common patterns: summarizing, reformatting, and explaining well-established concepts.

**Limitation:** Anything requiring precision on specifics. Claude can produce text that appears accurate but isn't. This risk concentrates around names, dates, citations, and statistics.

**Mitigation:** Use citations, uncertainty signaling, and generator-verifier loops. Route specific factual lookups through tool calls or authoritative sources rather than relying solely on the model's output.

## Knowledge

**Capability:** Topics in the model's training data that are common, recent, and consistently included where the model can answer reliably from what it learned.

**Limitation:** Topics that are rare, niche, contested, or frequently changing. The model may present stale or incomplete information with the same confident tone it uses for established facts.

**Mitigation:** Use web search, retrieval (RAG), tool use, or MCP servers to make an external system the source of truth instead of the model. Instead of checking the model's parametric knowledge, the authoritative answer comes from the retrieved source. When freshness or authority matters, re-introduce the data yourself rather than relying on what was provided in the model's training data.

## Working memory

**Capability:** Anything that fits in the active context window.

**Limitation:** The context window is a hard edge: once content falls outside the window, the model has no access to it at all. Two different errors happen at the edge, but they can be easy to conflate. One is an oversized request, meaning a prompt or conversation that is already too large to send. When an oversized request is sent it is rejected before generation. If the request exceeds the model's token limit, the API returns a `400 invalid_request_error` with a message indicating the prompt is too long. If the raw request body exceeds the API's byte limit, the API returns a `413 request_too_large` error with a message indicating the request exceeds the maximum allowed number of bytes. The other error occurs when a prompt fits, but its generation runs into the window ceiling and stops early instead; on current models the response comes back with a `model_context_window_exceeded` stop reason and truncated output. To avoid hitting the limit, you can check the `usage` field on every response and the token-counting API before you hit send.

**Mitigation:** Use progressive context loading, chunking, and front-loading of critical information. For extended work, projects can help manage what stays in scope. Get into the practice of summarizing across turns when context is getting long.

## Steerability

**Capability:** Short, concrete, and verifiable instructions with defined formats, explicit length limits, clear roles.

**Limitation:** Abstract or ambiguous instructions, long reasoning chains, and tasks requiring precise numerical or logical computation. For high-stakes numerical accuracy, deterministic computation or tool execution should own the answer. The model may follow the letter of an instruction while drifting from the intent.

**Mitigation:** Use system prompts, structured outputs, and code execution for anything requiring logical precision. When intent and literal instruction might diverge, restate the goal explicitly alongside the instruction.

## From property to design consequence

We will revisit each of these properties in a later part of this course. Map each property to their design consequence now so the connection is in place before you need it:

- **Non-determinism.** The same input can produce different outputs across runs. This is why evaluation frameworks exist: you cannot certify behavior you only observed once. (Feeds the evaluation work in Module 2.)
- **Context as a finite resource.** The context window is a hard edge with a fixed token budget. What you put in it, in what order, and what you leave out are design decisions that affect both what the model can work with and what it costs to run. (Feeds model and context strategy, later in this module.)
- **Confidence is not validity.** Claude can produce a wrong answer in the same fluent, assured tone it uses for a right one. This is why human-in-the-loop placement and verification are architectural choices, not afterthoughts. (Feeds the responsible-deployment work in Module 3.)
- **Knowledge and capability boundaries.** The model is reliable with topics that are common, recent, and consistent in its training data, and unreliable with topics that are rare, private, or fast-changing. For unreliable topics, web search, retrieval, tools, and MCP can be used to make an external system the source of truth instead of the model. (Feeds reference architectures and RAG, later in this module.)

> **Scenario: A failure that began with a misread property**
>
> An architect saw a demo run cleanly five times in a row and concluded the behavior was deterministic. On that basis the team shipped a financial reconciliation pipeline that treated each model output as a fixed, repeatable result and built no checks around it. In the second week of production the outputs drifted: the same statement, re-processed, produced a different categorization. This discrepancy was discovered by chance only when an analyst happened to re-run a batch. Nothing had changed in the input, but different outputs were produced because the model is a non-deterministic system and the architecture had been built as though it was deterministic.
>
> The lesson is not that the model is unreliable, it's that a demo is not evidence of determinism and that the four properties are present whether your architecture acknowledges them or not.

> **Cost · Complexity · Risk**
>
> **Cost:** Designing without considering these properties is the most expensive mistake an architect can make, because the cost lands after launch, when rework is most expensive and rebuilding trust is the hardest.
>
> **Complexity:** Naming the four properties up front keeps later design conversations precise. You can acknowledge "this is a knowledge-boundary problem" instead of debating if the model is "good enough."
>
> **Risk:** The properties do not announce themselves. A system that does not design around these properties won't produce an error; it drifts quietly and the gap results in variable outputs that may be found in an audit or by an angry user, not by the system itself.
