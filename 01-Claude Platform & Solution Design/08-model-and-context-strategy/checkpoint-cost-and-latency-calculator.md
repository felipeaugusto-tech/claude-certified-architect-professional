# 08 · Model & Context Strategy — Checkpoint: Cost & Latency Calculator

*Cost & latency calculator · 9 min*

Use the calculator to explore configurations. Set the model tier (Opus / Sonnet / Haiku), context strategy (monolithic / progressive), call volume per day, and extended thinking (on / off). The readouts show cost and latency for each setting. Your goal: find a configuration that meets both the cost ceiling and the latency budget at the same time, the two cannot be traded against each other. The calculator only displays values; it does not score your exploration. More than one valid configuration exists.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Calculator controls

- **Model tier:** Opus · Sonnet (default) · Haiku
- **Context strategy:** Monolithic · Progressive (default)
- **Calls per day:** number input (default 10,000)
- **Extended thinking:** Off (default) · On

**Readouts:** Estimated monthly cost · Median latency

*Illustrative figures based on published list pricing as of June 2026 (current per-million-token input/output pricing for Opus, Sonnet, and Haiku, available at docs.claude.com). Verify current pricing and latency at docs.claude.com before relying on these numbers with a partner.*

## Decision: identify the load-bearing control

### Question 1

Once you have a configuration where both readouts are within budget, ask: which single setting, if relaxed, would breach a budget first?

- **A.** The call volume, because it is the only input you cannot change.
- **B.** The dominant constraint's control, for example, if latency is the tight budget, the model tier or extended-thinking setting that drives latency.
- **C.** None; any setting can be changed freely once both readouts are green.

<details><summary>Answer</summary>

**B** — the load-bearing control is the setting tied to whichever budget has the least margin; volume is a given, and "change anything freely" ignores which budget is tight. *(See 08 · Model & Context Strategy → Model & Context Strategy → What extended thinking controls.)*

</details>

## Your rationale

Once you have a passing configuration and have selected the correct answer above: in 2–3 sentences, name the dominant constraint in your passing configuration and identify the single setting that is load-bearing for it. Write your answer, then reveal the model answer below.

*Name the dominant constraint and the single setting that is load-bearing for it…*

<details><summary>Model answer</summary>

The dominant constraint depends on which budget was tighter in your configuration. If latency is the binding constraint, the model tier and extended-thinking setting are load-bearing, those are the controls that most directly move latency. If cost is binding, the tier and context strategy are load-bearing. The load-bearing control is the one tied to whichever budget had the least margin. Naming it explicitly is what makes the configuration defensible rather than lucky.

</details>
