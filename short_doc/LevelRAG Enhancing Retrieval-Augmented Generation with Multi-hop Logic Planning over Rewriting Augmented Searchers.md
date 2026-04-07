# 📜Basic Information

> - **Link**: [[2502.18139] LevelRAG: Enhancing Retrieval-Augmented Generation with Multi-hop Logic Planning over Rewriting Augmented Searchers](https://arxiv.org/abs/2502.18139)
> - **comments**: 
> - **Tags**: #RAG #reason 

# 🧠 Core Idea

LevelRAG addresses a fundamental limitation of traditional Retrieval-Augmented Generation (RAG):
> Retrieval is treated as a one-shot operation rather than a structured reasoning process.

## Motivation

Existing RAG systems suffer from four key problems:
- **Inaccurate retrieval**: user queries are ambiguous or misaligned with retrievers
- **Incomplete retrieval**: single-pass retrieval fails for multi-hop questions
- **Query rewriting vs hybrid retrieval conflict**: rewriting is often tailored to dense retrievers and does not generalize to multiple retrievers
- **Naive context aggregation**: simply concatenating retrieved documents introduces noise and contradictions

👉 Therefore, the core problem is:
> Retrieval lacks **planning, decomposition, and verification**

## Core Shift

LevelRAG reframes RAG as:
- From: retrieve → generate
- To: plan → retrieve → summarize → verify → iterate

👉 Retrieval becomes a **multi-step reasoning process**

# 🔁 Pipeline
![[../pic/Pasted image 20260407200745.png]]
```plaintext
User Query
   ↓
[High-Level Searcher]
   ↓ (decompose)
Atomic Queries (q1, q2, ...)
   ↓
[Low-Level Searchers]
   ↓ (retrieve + rewrite)
Documents
   ↓
[High-Level Searcher]
   ↓ (summarize)
Summaries (s1, s2, ...)
   ↓
[High-Level Searcher]
   ↓ (verify)
   ├── enough → Generator → Answer
   └── not enough → supplement → new queries → loop
```

# 🧩 High-Level vs Low-Level

## High-Level Searcher (Planner)

Responsible for:
- Decompose: break complex query into atomic queries
- Summarize: convert retrieved docs into answers to sub-questions
- Verify: check if information is sufficient
- Supplement: generate new queries if needed

👉 It answers:
> “What information is still missing?”
👉 Retriever-agnostic
## Low-Level Searchers (Executors)

Responsible for:
- Query rewriting
- Retrieval execution
- Local optimization

Three types:
- Sparse searcher → keyword matching (BM25 / Lucene)
- Dense searcher → semantic retrieval (embeddings, pseudo-docs)
- Web searcher → external knowledge

👉 They answer:
> “How should this query be searched?”

# 🔗 Collaboration Mechanism

## Not naive hybrid retrieval
❌ Wrong:
```plaintext
query → multiple retrievers → concatenate
```
## LevelRAG approach
```plaintext
atomic query
   ↓
multiple searchers (parallel retrieval)
   ↓
documents (multi-source)
   ↓
high-level aggregation + summarization
```
## Key Insight

👉 Collaboration does NOT happen between retrievers
👉 It happens at the **high-level layer** via:
- aggregation
- summarization
- verification
- iterative planning

# ⚔️ Query Rewriting vs Hybrid Retrieval Conflict
## Problem
- Query rewriting is typically designed for **dense retrievers**
- Hybrid retrieval requires:
  - sparse (keyword)
  - dense (semantic)
  - web (external)
👉 A single rewritten query cannot satisfy all retrievers
## Consequence
- Dense-friendly rewrite → hurts sparse retrieval
- Sparse-friendly rewrite → loses semantic richness for dense retrieval
## LevelRAG Solution
👉 Decouple responsibilities:
```plaintext
High-level → task decomposition (what to search)
Low-level → retriever-specific rewriting (how to search)
```

# 🔑 Takeaways

- Retrieval is not a one-shot step, but an **iterative reasoning process** (plan → retrieve → verify → refine).
- The key idea is **decoupling**: high-level decides _what to search_, low-level decides _how to search_.
- Hybrid retrieval only works when **coordinated**, not when results are simply concatenated.
- **Atomic query decomposition** is essential for handling multi-hop questions.
- Retrieved documents must be transformed into **answer-oriented summaries**, not raw context.
- LevelRAG signals a shift from RAG to **retrieval-as-reasoning systems**.