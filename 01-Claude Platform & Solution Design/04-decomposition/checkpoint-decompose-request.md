# 04 · Decomposition — Checkpoint: Decompose Request

*Decompose the request · 4 min*

A partner brief is below. For each step, select the right owner: what Claude does, what existing systems do, or what humans do. The previous checkpoint tested whether you could recognize the four properties; this one tests whether you can decompose the split.

> **The brief**
>
> A regional logistics partner wants an assistant that, for each inbound shipping exception: reads the carrier's free-text exception note, decides whether the shipment qualifies for an automatic refund under the partner's published policy, looks up the customer's contract tier, drafts a notification to the customer, and issues the refund.

**Options for each step:** Claude · Existing System · Human

### Step 1

Read the carrier's free-text exception note

<details><summary>Answer</summary>

**Claude** — reading free-text is pattern-rich language work. *(See 04 · Decomposition → Decomposition.)*

</details>

### Step 2

Decide whether the shipment qualifies for an automatic refund under the partner's published policy

<details><summary>Answer</summary>

**Existing System** — a published policy is a deterministic rule the partner already maintains. *(See 04 · Decomposition → Watch Out: Deterministic Drift.)*

</details>

### Step 3

Look up the customer's contract tier

<details><summary>Answer</summary>

**Existing System** — contract tier is live data owned by the system of record. *(See 04 · Decomposition → Decomposition.)*

</details>

### Step 4

Draft the customer notification

<details><summary>Answer</summary>

**Claude** — drafting is language work. *(See 04 · Decomposition → Decomposition.)*

</details>

### Step 5

Issue the refund

<details><summary>Answer</summary>

**Human** — a refund is a high-stakes, hard-to-reverse action that needs approval and accountability. *(See 04 · Decomposition → Decomposition.)*

</details>
