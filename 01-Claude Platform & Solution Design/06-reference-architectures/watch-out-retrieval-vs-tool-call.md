# 06 · Reference Architectures — Watch Out: Retrieval vs Tool Call

*When retrieval got reached for instead of a tool call · 4 min*

> **Setup hook: how RAG fails when applied to live state**
>
> The broader underlying skill here is context engineering: deciding which mechanism is responsible for getting each kind of data in front of the model. Retrieval over a vector index is one example, and this trace shows what happens when it is asked to own data it cannot own. This often happens when a team reaches for *RAG* because it worked on a previous use case, without stopping to ask whether the new one shares the same properties. RAG works well when the answer lives in a body of reference material that remains stable between index refreshes, e.g., documentation, policy text, product manuals. It breaks for live state because retrieval treats your data as text snapshots, not as a system with current values. When those snapshots disagree, the model picks the most similar one and answers confidently. There is no way to know which snapshot is current. The trace below shows exactly how this plays out.

> **RAG** — A pattern where Claude retrieves relevant external content at request time and uses it to ground its response.

## The trace: electronics retailer customer service assistant

```text
User asks: "Where's my order?"

RETRIEVED CHUNK #1 (similarity 0.78): "Order #88412 was placed on the 11th of June and shipped on the 12th via standard courier. Tracking number AB123…"
RETRIEVED CHUNK #2 (similarity 0.74): "Order #88412 is currently being processed. Expected dispatch within 24 hours."

MODEL OUTPUT: "Your order shipped on the 12th of June via standard courier. Tracking: AB123…"
```

Both chunks were real strings that existed in the corpus at different points in time. The index conflated them. The order had shipped, been returned to depot due to a damaged label, and was awaiting re-dispatch. Note that this current state appears in neither chunk: the corpus held two stale snapshots and no record of where the order was, because an index captures what was true when it was written, not at the present. A customer service tool to fetch live order status existed in the partner's API. It was not called.

## What broke and why

### The category error

Retrieval is the right mechanism for knowledge: FAQs, policies, manuals. It's the wrong mechanism for transactional state. Order status wasn't failing because retrieval is broken. It was failing because current state had been represented as historical text snapshots in the first place.

### A data-architecture failure, not a retrieval failure

Live state was indexed as text, so the system searched a corpus of past snapshots rather than querying the system of record.

### Similarity is not truth

Embedding similarity confidently merged two stale snapshots into one answer. A higher similarity score does not mean a truer answer; it means the retrieved text was semantically close to the query, which is not the same thing when the underlying state has changed since the text was written.

### The fix is a tool call

Not a better chunker, a shorter refresh interval, or a higher similarity threshold: a tool call to the order-status service. The knowledge base keeps the FAQ content. The transactional database keeps the orders. Two types of data, two access patterns, two mechanisms.

## The retrieval principle

Retrieval is for stable knowledge: things that were true yesterday and will be true tomorrow. Tool use is for live state: things whose current value is owned by a system and changes independently of your index. Conflating them produces answers that are fluent, confident, and wrong in ways that are hard to detect because the system shows no error signal. The model returned a response. The response looked correct. The customer got false information about their own order.
