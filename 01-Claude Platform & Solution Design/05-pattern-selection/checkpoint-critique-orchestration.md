# 05 · Pattern Selection — Checkpoint: Critique Orchestration

*Critique the orchestration design · 5 min*

> **Draft architecture submitted for review**
>
> Below is a draft *multi-agent* architecture submitted for review: an orchestrator fanning a large document-classification job out to subagents. Six components are listed. Select the three that carry a control or failure-boundary defect.

> **Multi-agent** — An architecture where multiple Claude instances (agents) each handle a subtask, coordinated by an orchestrating layer.

Select **exactly 3** components that carry a control or failure-boundary defect.

### Components

1. Orchestrator decomposes the corpus into per-document units
2. Subagents run in parallel, each returns a verdict
3. Synthesis sums returned verdicts into a report
4. Irreversible action (auto-archive) taken with no human gate
5. No retry or gap-flag on a failed subagent
6. Shared trace ID propagated to every subagent

*(Flow: 1 → 2 → 3 → 4; components 5 and 6 sit alongside the flow.)*

<details><summary>Answer</summary>

**3, 4, 5**

- **3** — synthesis sums only the verdicts it received, with no coverage check that results equal units dispatched.
- **4** — an irreversible action needs a human-in-the-loop gate before it runs.
- **5** — a failed subagent is the recoverable case only if something retries it or flags the gap.

Components 1, 2 and 6 are sound design (6 is the recommended observability fix). *(See 05 · Pattern Selection → Multi-Agent Systems and Watch Out: Fan-out Drop.)*

</details>
