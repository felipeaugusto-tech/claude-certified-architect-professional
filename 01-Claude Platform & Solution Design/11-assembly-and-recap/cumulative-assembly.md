# 11 · Assembly & Recap — Cumulative Assembly

*A contract-review system for a mid-market law firm · 7 min*

Four complete architectures are described below. Each one commits to all five design decisions the module covered: the platform *entry point*, the pattern, how the work is split across Claude, existing systems, and humans, the model and context strategy, and the human-in-the-loop posture. Only one holds up against the brief. The other three each look reasonable but fail on a single decision. Pick the one you would put in front of a partner-side review committee.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

> **Entry point** — The interface through which end users or systems access Claude in production (e.g. claude.ai, a custom app, an API integration).

> **The brief**
>
> A 180-lawyer mid-market law firm wants to speed up contract review. Their senior associates currently spend an estimated 12 to 18 hours per week reading vendor and partnership contracts to flag clauses that conflict with the firm's standard playbook. Average contract length is 35 pages. The current output is a redlined PDF with margin comments. The firm uses iManage for document storage, has a private LLM gateway approved by their CIO, and is bound by attorney-client privilege requirements that exclude consumer-grade tools. The target is to cut associate time per contract by 60% while keeping the senior associate as the final reviewer.

## Step 1: draft your architecture

Before reading the four options, draft your own architecture for the law firm. In one paragraph, cover all five decisions: the platform entry point, the pattern, how the work is split across Claude and existing systems, the model and context strategy, and the human-in-the-loop posture.

*Click-to-reveal an answer to compare with your response; feel free to ask Claude to compare what you've written with the provided answer.*

*Cover all five decisions: platform entry point, pattern, work split, model/context strategy, and human-in-the-loop posture…*

<details><summary>Model answer</summary>

Build on the direct API or SDK, embedded in a thin internal web app that authenticates via the firm's SSO and routes through the approved LLM gateway. A parallelized workflow reviews the contract section by section, with an evaluator pass enforcing a strict schema on the flagged-clauses output. Claude handles extraction, classification against the playbook, and draft redlines. The playbook stays in the firm's systems as a versioned source of truth, retrieved per clause at call time. iManage handles document fetch. Sonnet is the default with progressive context, extended thinking enabled per clause only where an eval set justifies it. The senior associate signs off on every output, and low-confidence clauses are flagged for attention.

</details>

## Step 2: which of the four options matches your design most closely?

### Question 1

- **Option A.** Build on Claude.ai, with associates uploading each contract into a Project that holds the playbook as reference files. A parallelized workflow reviews the contract section by section and aggregates the flagged clauses. Sonnet is the default model with progressive context. The senior associate reviews every output before anything goes to a client.
- **Option B.** Build on the direct API or the SDK, embedded in a thin internal web app that authenticates via the firm's SSO and routes through the approved LLM gateway. The playbook is loaded into the system prompt in full on every call so the model always has the firm's standard in context. A parallelized workflow reviews the contract by section and aggregates results. Sonnet is the default. The senior associate reviews every output.
- **Option C.** Build on the direct API or the SDK, embedded in a thin internal web app behind SSO and the approved gateway. An open-ended agent is given the contract and the iManage tools and left to decide for itself how to work through the document. The playbook is retrieved per clause at call time. Opus runs on every call for maximum accuracy. The senior associate reviews every output.
- **Option D.** Build on the direct API or the SDK, embedded in a thin internal web app that authenticates via SSO and routes through the approved gateway. A parallelized workflow reviews the contract section by section, with an evaluator pass enforcing a strict schema on the flagged-clauses output. Claude handles extraction, classification against the playbook, and draft redlines. The playbook stays in the firm's systems as a versioned source of truth, retrieved per clause at call time. iManage handles document fetch. Sonnet is the default with progressive context, extended thinking enabled per clause only where an eval set justifies it. The senior associate signs off on every output, and low-confidence clauses are flagged for attention.

<details><summary>Answer</summary>

**D** — it matches the model answer on all five decisions. A fails the entry point (privilege excludes consumer-grade tools), B fails context strategy (monolithic playbook on every call), and C fails pattern and model choice (open-ended agent for an enumerable task, Opus everywhere). *(See 10 · Entry Points & Governance → Entry Point & Route Selection; 08 · Model & Context Strategy; 05 · Pattern Selection.)*

</details>
