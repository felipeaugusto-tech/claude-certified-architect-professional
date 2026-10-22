# 04 · Decomposition — Checkpoint: Sort Capabilities

*Sort the field-service capabilities · 4 min*

A field-service partner has handed you a request list for a knowledge assistant their engineers will use on-site. Click a capability to select it, then click the right owner bucket to place it. All 8 items must be placed before submitting.

**Buckets:** Claude · Existing Systems · Human

### Capabilities

1. Summarize the engineer's case notes into a one-page handover
2. Return the current stock level of part SKU 78-A at the closest warehouse
3. Approve a refund above £2,000 if the engineer requests one
4. Extract the part number from a photo of the unit label
5. Calculate the total billable time across three job tickets
6. Draft a follow-up email to the customer explaining the delay
7. Tell the engineer whether the warranty applies to this serial number
8. Decide whether to escalate a safety incident to the field manager

<details><summary>Answer</summary>

| Capability | Owner |
|---|---|
| Summarize the engineer's case notes into a one-page handover | Claude |
| Return the current stock level of part SKU 78-A at the closest warehouse | Existing Systems |
| Approve a refund above £2,000 if the engineer requests one | Human |
| Extract the part number from a photo of the unit label | Claude |
| Calculate the total billable time across three job tickets | Existing Systems |
| Draft a follow-up email to the customer explaining the delay | Claude |
| Tell the engineer whether the warranty applies to this serial number | Existing Systems |
| Decide whether to escalate a safety incident to the field manager | Human |

Language work (summarize, extract, draft) goes to Claude; live state and precise computation (stock, billable time, warranty) belong to the systems of record; high-stakes judgment calls and approvals stay with a human. *(See 04 · Decomposition → Decomposition.)*

</details>
