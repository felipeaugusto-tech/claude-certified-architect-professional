# 04 · Sizing — Justify the feasibility call

*Justify the feasibility call · 5 min*

For each scenario, choose the option that names both the correct feasibility verdict and the single load-bearing constraint behind it. The verdict alone is not enough, the constraint is what makes the verdict defensible.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Scenario 1

A professional-services firm wants a research assistant that summarizes 10–40 page industry reports and drafts client briefings. 50 reports per week, briefings within 24 hours, $500/month ceiling.

- **A.** Not feasible, the reports are too long for the context window.
- **B.** Feasible as scoped, input size fits the context window, and volume, latency, and cost are all within range at the Sonnet tier; no AI property presents a disqualifying constraint.
- **C.** Feasible with constraints, needs a human review gate on every briefing.

<details><summary>Answer</summary>

**B** — 10–40 pages fits the window, and low volume, a 24-hour SLA and a modest budget leave no property needing a compensating control. *(See 04 · Sizing → Sizing and feasibility → Feasibility verdicts.)*

</details>

### Scenario 2

A logistics company wants a delay predictor that reads unstructured carrier emails, extracts delay reasons and new ETAs, and writes them to the order-management system. 5,000 emails/day, under 10 seconds, $1,000/month.

- **A.** Feasible as scoped, Haiku with caching handles the volume and latency.
- **B.** Not feasible, the volume is too high.
- **C.** Feasible with constraints, the load-bearing constraint is extraction accuracy on a transactional write, so it requires a code-based eval on extraction accuracy plus a human review gate on low-confidence extractions before writing to the system of record.

<details><summary>Answer</summary>

**C** — extracting specific values and writing them to a system of record needs verification (next-token prediction property), so the verdict carries constraints. *(See 04 · Sizing → Sizing and feasibility → Next-token prediction.)*

</details>

### Scenario 3

A financial-services firm wants real-time trading recommendations from current market conditions plus its proprietary model, delivered in under 2 seconds.

- **A.** Not feasible as described, the load-bearing constraint is the live-state knowledge gap: real-time market data requires a tool call to a live feed, and whether that round trip fits inside the 2-second budget must be validated before any feasibility verdict can be issued.
- **B.** Feasible as scoped, Claude already knows current market conditions.
- **C.** Feasible with constraints, just add a human gate.

<details><summary>Answer</summary>

**A** — live market state can't come from training data, and the tool-call round trip against a 2-second budget is unvalidated. *(See 04 · Sizing → Sizing and feasibility → Knowledge.)*

</details>
