# 07 · RAG Pipeline Design — RAG Pipeline Design

*Chunking and indexing · 10 min*

**The previous screen on reference-architectures named RAG as a known-good shape and showed where it is misapplied. This screen goes one level deeper, into the design of the retrieval pipeline itself: how a corpus is broken into chunks, how those chunks are indexed, and how the retrieval strategy is matched to the query patterns the system will see.**

## Chunking: how you break the corpus, by what the corpus is

A chunk is the unit that gets retrieved. The chunking approach is chosen by the structure of the source material, not by a default size.

| Chunking approach | How it works | When it earns its place |
|---|---|---|
| **Fixed-size** | Split into uniform spans (with overlap) regardless of structure. | Homogeneous, unstructured text where natural boundaries are weak; simplest to operate. |
| **Semantic** | Split on meaning boundaries, topic shifts, sentence groups that hang together. | Prose where a retrieved chunk must be self-contained to answer well; reduces mid-idea cuts. |
| **Hierarchical** | Preserve document structure, sections, subsections, and retrieve at the level that fits. | Structured documents (contracts, manuals, policies) where section context carries meaning. |

## Indexing: how you make chunks findable, by how queries are phrased

Indexing decides what "similar" means when a query arrives. The strategy is chosen by the query pattern.

| Indexing strategy | What it matches | When it earns its place |
|---|---|---|
| **Dense (embeddings)** | Semantic similarity, meaning, not words. | Queries phrased differently from the source; paraphrase, intent, concept matching. |
| **Sparse (keyword, e.g. BM25)** | Exact terms, identifiers, codes, names. | Queries that hinge on specific tokens: part numbers, statute citations, error codes. |
| **Hybrid** | Both, with the results combined. | Mixed query patterns, the common production case; recovers the exact-match results dense retrieval misses. |

When a hybrid index returns two ranked lists, they must be merged into one. Reciprocal rank fusion is the standard, low-tuning way to do it: each result is scored by its rank in each list, and the combined score favors items that rank well in both. The point for an architect is combining dense and sparse results is a design decision with a known, defensible default.

## Articulating the trade-off

Every retrieval design makes trade-offs among three things:

- **Retrieval quality:** Does the right chunk come back?
- **Latency:** How long does retrieval add to each request?
- **Maintenance:** How much does the pipeline cost to keep correct as the corpus grows and changes?

Smaller chunks and hybrid indexing tend to raise quality and latency together; larger chunks and dense-only indexing lower latency and maintenance but miss exact-match queries. There is no universally right point, only the point that fits this corpus and these queries, stated as a trade-off you can defend.

> **Cost · Complexity · Risk**
>
> **Cost:** Hybrid indexing and smaller chunks raise both retrieval compute and per-request latency. Size the pipeline to the query patterns you actually have, not to the most thorough one imaginable.
>
> **Complexity:** Every chunking and indexing choice is something to maintain as the corpus changes. A pipeline that was correct at launch can degrade silently as documents are added.
>
> **Risk:** The failure mode is a confident answer built on the wrong chunk. Retrieval quality is not visible in the output, it has to be measured against a labeled set, which is why this work ties straight to evaluation.
