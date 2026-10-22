# 06 · A/B & Obs — Experiment-design plane

*Place on the experiment-design plane · 5 min*

The plane below has two axes: the **expected effect size** (how large a difference you expect to see) and the **confidence requirement** (how certain you need to be before acting on the result). Place each scenario in the correct zone. The correct placement determines the right experimental posture, which in turn determines the minimum sample size.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Zones

| Confidence ↓ / Effect → | Small effect | Large effect |
|---|---|---|
| **High confidence** | Small effect · High confidence | Large effect · High confidence |
| **Moderate confidence** | Small effect · Moderate confidence | Large effect · Moderate confidence |
| **Low confidence** | Small effect · Low confidence | *(not used in this exercise)* |

### Scenarios

- **A.** A minor wording change to a clarification message in a low-stakes FAQ chatbot.
- **B.** A prompt architecture change for a medical intake summarizer where an error could delay treatment.
- **C.** Adding a new classification category to a routing model expected to capture 30% of incoming volume.
- **D.** Testing whether switching from Sonnet to Haiku on a simple formatting task saves cost without degrading quality.
- **E.** A small change to the retrieval prompt in a RAG system processing 200 requests per day.

<details><summary>Answer</summary>

| Scenario | Zone | Why |
|---|---|---|
| **A** | Small effect · Low confidence | A minor wording tweak in a low-stakes bot; being wrong costs little. |
| **B** | Large effect · High confidence | An architecture-level change where an error could delay treatment. |
| **C** | Large effect · Moderate confidence | Shifts 30% of volume, but routing is recoverable. |
| **D** | Small effect · High confidence | You're proving *no* quality loss, which needs a tight confidence bound on a small difference. |
| **E** | Small effect · Moderate confidence | A small tweak at only 200 requests/day limits how much confidence you can buy. |

The five scenarios map one per zone. *(See 06 · A/B & Obs → A/B testing and observability → Structured A/B testing.)*

</details>
