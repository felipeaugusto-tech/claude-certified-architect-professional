# 07 · Exercise: Eval Framework

*Exercise: define the evaluation framework · 8 min*

> **The brief**
>
> A regional insurer is deploying a Claude system that reads a submitted claim, extracts structured fields (claimant, policy number, loss amount, date of loss), summarizes the narrative for an adjuster, and flags claims that may warrant fraud review. The system must respond within a few seconds, stay within a defined per-claim cost, never leak one claimant's data into another's summary, and never auto-deny a claim.

Draft the evaluation framework for this system. For each of the five dimensions below, write the metric, the grading method (code-based eval, LLM judge, or human review), and a one-sentence reason for your choice. Write your framework, then reveal the model answer below.

*[Click-to-reveal an answer to compare with your response; feel free to ask Claude to compare what you've written with the provided answer.]*

## Your framework

1. **Accuracy: field extraction** — *Metric, grading method, and a one-sentence reason…*
2. **Latency: response time** — *Metric, grading method, and a one-sentence reason…*
3. **Safety: summary faithfulness and no auto-deny** — *Metric, grading method, and a one-sentence reason…*
4. **Security: no cross-claimant data leakage** — *Metric, grading method, and a one-sentence reason…*
5. **Cost: per-claim spend** — *Metric, grading method, and a one-sentence reason…*

<details><summary>Model answer</summary>

**1. Accuracy, field extraction:** Code-based eval. The expected values (claimant name, policy number, loss amount, date of loss) are known and verifiable by exact or schema match. No interpretation required; a function checks the output against the ground truth.

**2. Latency, response time:** Code-based eval. Latency p95 is a number. The check is whether it falls below the target. No judgment involved.

**3. Safety, summary faithfulness and no auto-deny:** Two methods required. Code-based eval for the deny action (binary, either a denial was issued or it was not). LLM judge for summary faithfulness (whether the narrative accurately represents the source claim without fabrication is an interpretive task a function cannot encode).

**4. Security, no cross-claimant data leakage:** Code-based eval. Cross-claimant leakage can be checked by scanning each summary for identifiers that appear in any input claim other than the one being processed. Deterministic check, no interpretation needed.

**5. Cost, per-claim spend:** Code-based eval. Cost is a numeric value derived from input tokens, output tokens, model tier, and whether prompt caching applies. The check is whether it exceeds the ceiling.

</details>

<details><summary>Self-assessment</summary>

- **Correct:** You applied the grading ladder correctly across all five dimensions. The distinguishing moves: splitting Dimension 3 into code for the action and judge for faithfulness, and keeping Security in code because the check is deterministic despite the high stakes.
- **Partially correct:** At least one dimension is mismatched. The most common errors: using a judge for field extraction (it's deterministic, code handles it), and treating the auto-deny check as interpretive (it's binary, code handles it). Re-read the model-based vs. code-based evals table and revise.
- **Incorrect:** Re-read the grading ladder: use code where the behavior is unambiguous (known value, binary check, numeric threshold); use a judge where correctness requires interpretation. Apply that question to each dimension and redo.
- **Skip:** Move on if you need to, but the cumulative task asks you to design an eval strategy from scratch. Come back before then.

</details>
