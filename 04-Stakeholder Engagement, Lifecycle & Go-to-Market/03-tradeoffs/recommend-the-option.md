# 03 · Tradeoffs — Recommend the option

*Checkpoint: recommend the option and name the missing element*

Try it now. Read the one-paragraph stakeholder briefing and the three option presentations written in plain language. One option is technically accurate and presented comprehensively. One is technically accurate, but its presentation is missing an element. One is inappropriate for the stated constraints. Recommend the option to put forward, then name the single element missing from the second option's presentation.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

> **Briefing**
>
> A mid-sized insurer wants Claude to draft adjuster responses to policyholder queries. The workflow is covered by state insurance regulation with an audit-trail obligation. Volume is high and steady. The sponsor cares about response quality and about keeping a defensible record of every automated interaction.

| Option | Presentation as shown |
|---|---|
| **A** | Workflow pattern with per-interaction logging built in. Gains: full audit trail, quality gate before sending. Gives up: a small latency cost from the logging step. Reversal: minor, logging can be tuned without redesign. |
| **B** | Workflow pattern that trades the logging step for lower latency. Gains: faster responses. Gives up: per-interaction audit detail. |
| **C** | Single augmented call with no logging and no human gate, chosen for lowest build cost. |

### Part 1

Which option should you recommend?

- **A.** Option A, logging built in, full audit trail
- **B.** Option B, trades logging for lower latency
- **C.** Option C, no logging, no human gate

<details><summary>Answer</summary>

**A** — the audit-trail obligation makes per-interaction logging load-bearing, and A is the only option presented with all three elements (gain, give-up, reversal). C fails the stated constraints outright. *(See 03 · Tradeoffs → Present a tradeoff so stakeholders can act.)*

</details>

### Part 2

What single element is missing from Option B's presentation?

- **A.** What Option B gains (faster responses is not named clearly enough)
- **B.** What Option B gives up (audit detail loss is not explained)
- **C.** The reversal cost, what it costs to restore logging after the design depends on the latency gain
- **D.** The compliance posture this option creates

<details><summary>Answer</summary>

**C** — B names its gain and its give-up but never says what restoring logging would cost once the system depends on the latency gain, the element the module calls the one that changes the meeting. *(See 03 · Tradeoffs → Watch Out: approval was not an informed choice.)*

</details>
