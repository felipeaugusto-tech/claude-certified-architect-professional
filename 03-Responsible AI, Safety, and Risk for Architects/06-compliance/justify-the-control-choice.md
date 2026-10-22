# 06 · Compliance — Justify the control choice

*Justify the control choice · 3 min*

**Try it now.** You are given one compliance obligation for a regulated deployment. Pick the technical control that evidences it, then select the single load-bearing reason a security and legal reviewer would accept that control as proof. You pass only when both land: the control must fit the obligation, and you must name why it counts as evidence a reviewer can inspect.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Question 1

**Obligation:** Protected health data must be handled under a formal agreement (HIPAA).

- **A.** A HIPAA-ready plan or first-party API under a signed BAA, because the signed agreement plus the enabled configuration is the artifact a reviewer can inspect as proof.
- **B.** A HIPAA-ready plan under a signed BAA, because the model is instructed to handle health data carefully.
- **C.** A model-based content filter on health terms, because it catches sensitive data at runtime.

<details><summary>Answer</summary>

**A** — right control and right reason: the signed BAA plus the enabled configuration is inspectable evidence. B has the right control but a reason that is an instruction, not proof; C doesn't satisfy the agreement obligation at all. *(See 06 · Compliance → Compliance obligation to control → Mapping obligations.)*

</details>
