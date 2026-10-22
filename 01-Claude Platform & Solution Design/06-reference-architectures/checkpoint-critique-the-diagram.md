# 06 · Reference Architectures — Checkpoint: Critique the Diagram

*Critique the diagram · 6 min*

> **Practice: review a partner's draft architecture**
>
> Below is a reference-architecture sketch of a customer-service assistant. Six components are listed. Select the three that are misapplied for this routing design.

Select **exactly 3** components that are misapplied.

### Components

1. Intent classifier (Claude call)
2. Retrieval over "Order Status Index"
3. Retrieval over "Product Manual Corpus"
4. Agent loop with tool entry point: refund, cancel, update-address
5. (missing) Escalation path to human agent
6. Response composer (Claude call)

*(Flow: 1 → {2, 3} → {4, 5} → 6.)*

<details><summary>Answer</summary>

**2, 4, 5**

- **2** — order status is live state; it needs a tool call to the order API, not retrieval over an index.
- **4** — high-consequence actions (refund, cancel) belong behind a human approval layer, not in an autonomous agent loop, and an agent variant should not ship before the simpler routed workflow is measured.
- **5** — the routing design has no escalation path to a human.

Components 1, 3 and 6 fit the routing reference architecture (retrieval suits a stable product-manual corpus). *(See 06 · Reference Architectures → Reference Architectures and Watch Out: Retrieval vs Tool Call.)*

</details>
