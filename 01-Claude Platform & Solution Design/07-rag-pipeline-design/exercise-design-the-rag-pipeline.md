# 07 · RAG Pipeline Design — Exercise: Design the RAG Pipeline

*Design the RAG pipeline · 8 min*

> **The brief**
>
> A professional-services partner has a corpus of roughly 4,000 documents: client engagement contracts (highly structured, section-numbered), past project write-ups (long-form prose), and a methodology handbook (structured, with defined procedures). Their team asks three kinds of question: "what does our methodology say about X?" (concept lookup), "find the clause about termination in the Acme contract" (exact-target lookup), and "summarize how we've handled engagements like this one" (broad synthesis across write-ups).

Draft your pipeline design. For each of the three document types, name the *chunking* approach you would use and explain why. Then name the indexing strategy for the full corpus and state the trade-off you are making. Write your answer before clicking to reveal the model answer.

*Feel free to ask Claude to compare what you've written with the provided answer.*

> **Chunking** — Splitting source documents into smaller pieces before indexing, sized to balance retrieval precision against context cost.

## Your pipeline design

*Name the chunking approach for each document type and why, then the indexing strategy for the full corpus and the trade-off you are making…*

<details><summary>Model answer</summary>

**Contracts and methodology handbook:** Hierarchical chunking that preserves section and subsection structure. Both sources are structured and section-numbered, so retrieving at the section level keeps the clause or procedure intact and self-contained. Fixed-size chunking cuts across section boundaries and loses the structural context the query depends on.

**Project write-ups:** Semantic chunking on meaning boundaries so each retrieved chunk is self-contained. Long-form prose has no section numbers to anchor hierarchical chunking, and fixed-size chunks cut mid-idea. Semantic chunking keeps each retrieved passage coherent enough to answer the question on its own.

**Indexing strategy:** Hybrid dense-plus-sparse. The query set mixes exact-clause lookups ("find the termination clause in the Acme contract") with open-ended conceptual questions ("how did we approach X?"). Sparse handles exact-target lookups where specific identifiers matter; dense handles concept-lookup and synthesis queries where meaning, not exact terms, drives the match. Neither alone covers both patterns.

**The dominant trade-off:** query variety is the load-bearing constraint. The hybrid index adds retrieval compute and a rank-fusion step, but those costs earn their place because the query set genuinely needs both modes.

</details>

**How did your design compare?** Correct: my design matched · Partial: close, but off on one piece · Incorrect: I missed the structure
