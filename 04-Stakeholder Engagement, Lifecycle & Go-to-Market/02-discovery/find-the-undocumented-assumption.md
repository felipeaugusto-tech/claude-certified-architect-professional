# 02 · Discovery — Find the undocumented assumption

*Checkpoint: find the undocumented assumption*

Try it now. Below is a discovery-call summary and the requirements document an Architect produced from it. Three items trace to something the stakeholder said; one is an assumption the Architect made without surfacing the constraint underneath it. Choose the item that is the undocumented assumption, then confirm that no stakeholder statement supports it.

> **Note:** the course page doesn't include the answers. The answers below are inferred from the teaching content, not the official key.

### Requirements document

1. Claude drafts the customer email, but a person actually sends it.
2. Responses must return within a two-second perceived budget.
3. Refunds above the threshold route to a human approver.
4. Conversation transcripts are retained for sixty days for analytics.

### What the stakeholder said

- "Anything big requires a person's sign off."
- "Draft the reply, but we send it ourselves."
- "It has to feel instant to the user."

### Part 1

Which item is the undocumented assumption?

- **A.** Item 1, email drafting with human send
- **B.** Item 2, two-second latency budget
- **C.** Item 3, refund routing to human approver
- **D.** Item 4, sixty-day transcript retention

<details><summary>Answer</summary>

**D** — items 1–3 each trace to a stakeholder statement ("we send it ourselves", "feel instant", "anything big requires sign off"); nothing anyone said mentions retention or analytics. *(See 02 · Discovery → A discovery call is structured elicitation → The output of discovery is a translation table.)*

</details>

### Part 2

Which stakeholder statement should item 4 trace back to?

- **A.** "Draft the reply, but we send it ourselves."
- **B.** "It has to feel instant to the user."
- **C.** "Anything big requires a person's sign off."
- **D.** No stakeholder statement supports this item.

<details><summary>Answer</summary>

**D** — the retention rule is an unsourced assumption; it should be logged as an assumption and confirmed (with compliance, in this case) rather than written as a requirement. *(See 07 · Wrap-up → Glossary → Requirement vs assumption.)*

</details>
