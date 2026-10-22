# 04 · Feedback Loops — Triage the production signals

*Checkpoint: triage the production signals*

Try it now. Here are nine signals from a production deployment. Drag each into the bucket where it belongs: Internal monitoring, Architect review, Stakeholder review, or Noise.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

**Buckets:** Internal monitoring · Architect review · Stakeholder review · Noise

### Signals

1. Latency p99 ticked up 40ms, still inside budget.
2. Eval score down three weeks running, trend is clear.
3. Scheduled data-residency confirmation is due.
4. One malformed request from a known bad client.
5. Cost per interaction crossed the agreed budget threshold.
6. A 2 a.m. batch job logged a retry that then succeeded.
7. Quarterly output audit against the documentation standard is due.
8. Token usage rose with a known seasonal traffic bump.
9. A new prompt template shipped, and error rate is flat.

<details><summary>Answer</summary>

| Signal | Bucket | Why |
|---|---|---|
| 1. p99 up 40ms, inside budget | Internal monitoring | Within the SLA budget; watch it, no review needed. |
| 2. Eval score down three weeks running | Architect review | Sustained quality drift: diagnose prompt vs. data vs. model drift before escalating. |
| 3. Scheduled data-residency confirmation | Stakeholder review | A regulated checkpoint that fires on schedule, regardless of metrics. |
| 4. One malformed request from a known bad client | Noise | Isolated, explained, no trend. |
| 5. Cost per interaction crossed the agreed budget | Stakeholder review | The budget was agreed in discovery; revisiting it is a tradeoff for the stakeholder. |
| 6. Batch retry that then succeeded | Noise | Transient and self-resolved. |
| 7. Quarterly output audit due | Stakeholder review | Scheduled compliance checkpoint, independent of eval scores. |
| 8. Token usage rose with known seasonal traffic | Internal monitoring | Explained by a known pattern; track cost impact. |
| 9. New prompt template shipped, error rate flat | Internal monitoring | A change worth watching for quality drift, but no signal yet. |

Item 3 is the least certain: the governance table assigns the residency confirmation to the Architect ("confirm and record"), so *Architect review* is also defensible. *(See 04 · Feedback Loops → Feedback loop and SLA → Production-signal governance table.)*

</details>
