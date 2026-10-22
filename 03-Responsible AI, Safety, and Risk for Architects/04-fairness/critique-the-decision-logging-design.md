# 04 · Fairness — Critique the decision-logging design

*Critique the decision-logging design · 3 min*

**Try it now.** You are looking at a system sketch for a decision-support flow. Click the components where transparency is missing or insufficient, such as a routing step that is never logged or a decision that cannot be reconstructed. Each click reveals what that gap means for an affected user or a regulator.

*Select all gaps. Leave adequately logged components unselected.*

### Components

*(Diagram flow, top to bottom: 1 → 2 → 3 → 4; components 5 and 6 sit alongside the flow.)*

1. A routing step that sends some cases down a different path with no log entry.
2. A retrieval step whose returned context is not captured.
3. A model output stored without the inputs that produced it.
4. Inputs, outputs, and routing are all logged and tied to a session ID.
5. The log is queryable per decision and retained for 90 days.
6. An aggregate accuracy dashboard with no per-subgroup breakdown.

<details><summary>Answer</summary>

**Gaps: 1, 2, 3, 6** · **Adequate: 4, 5**

- **1** — an unlogged routing step hides the downstream-routing injection point; nobody can show why a case took a different path.
- **2** — without the retrieved context, a skewed corpus can be neither proven nor ruled out.
- **3** — an output stored without its inputs can't be reconstructed or explained to the affected user.
- **6** — an aggregate metric can look fine while harm concentrates in one subgroup.

*(See 04 · Fairness → Where unequal outcomes enter.)*

</details>
