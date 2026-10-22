# 03 · POC to Prod — Cost & reliability calculator

*Cost & reliability calculator · 5 min*

Use the calculator to explore configurations. Adjust the model tier, prompt caching, `max_tokens` cap, and call-volume multiplier; the readouts show monthly cost and p95 latency for each setting. Your goal: find a configuration that meets both the cost ceiling and the latency target at the same time. The calculator only displays values; it does not score your exploration. (Figures are representative for modeling; confirm current rates at publish time.)

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Scenario

A customer service agent processes **50,000 requests per month**. The system prompt is **5,000 tokens** and stable across requests. Average user input is **300 tokens**, average output is **400 tokens**. The **cost ceiling is $800/month**. The **p95 latency target is 3 seconds**.

### Calculator controls

- **Model tier:** Haiku · Sonnet (default) · Opus
- **Prompt caching:** On (default) / Off
- **`max_tokens` cap:** 128–2048 (default 512)
- **Call-volume multiplier:** 0.5×–3× (default 1.0×)

**Readouts:** Est. monthly cost (ceiling: $800/mo) · p95 latency (target: ≤3s)

### Question 1

Decision: Once you have a configuration that meets both targets, which lever did the most work to get there?

- **A.** Switching to Opus, because the most capable model is always safest.
- **B.** Turning prompt caching on, because the 5,000-token system prompt is stable across all 50,000 requests, so caching it cuts the dominant input-cost driver.
- **C.** Raising the `max_tokens` cap, because more headroom improves quality.

<details><summary>Answer</summary>

**B** — the 5,000-token stable prefix dwarfs the 300-token input, so caching attacks the dominant cost driver; Opus raises cost and latency, and a higher `max_tokens` cap only adds headroom for cost. *(See 03 · POC to Prod → From POC to production → Cost and latency modeling.)*

</details>
