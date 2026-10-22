# 02 · Evals — Sort the eval types

*Sort the eval types · 5 min*

Eight evaluation tasks are listed below. Drag each one into the bucket where it belongs: model-based eval or code-based eval. The right placement follows whether the behavior being checked requires interpretation or is straightforward.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

**Buckets:** Code-based eval · Model-based eval

### Evaluation tasks

1. Check that every response is valid JSON matching the defined output schema.
2. Assess whether the model's reasoning in a complex analysis is sound and complete.
3. Verify that the response length is under 500 tokens.
4. Score how appropriately the system handles an emotionally charged customer complaint.
5. Confirm that the extracted claim number matches the quantitative value in the source document.
6. Evaluate whether the summary captures the most important points from a long briefing document.
7. Verify that all required sections (executive summary, methodology, findings, recommendations) are present in the output.
8. Assess whether the tone of a customer-facing message is appropriately professional for the brand.

<details><summary>Answer</summary>

| Task | Bucket |
|---|---|
| 1. Valid JSON matching the schema | Code-based eval |
| 2. Reasoning is sound and complete | Model-based eval |
| 3. Response length under 500 tokens | Code-based eval |
| 4. Handling an emotionally charged complaint | Model-based eval |
| 5. Extracted claim number matches the source | Code-based eval |
| 6. Summary captures the most important points | Model-based eval |
| 7. All required sections present | Code-based eval |
| 8. Tone is appropriately professional | Model-based eval |

Unambiguous checks (schema, length, exact match, presence) go to code; anything needing interpretation (reasoning, appropriateness, importance, tone) goes to a judge. *(See 02 · Evals → Evals as acceptance criteria → The grading ladder.)*

</details>
