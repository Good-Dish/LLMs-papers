# 📜Basic Information

> - **Link**: [[2602.02007] Beyond RAG for Agent Memory: Retrieval by Decoupling and Aggregation](https://arxiv.org/abs/2602.02007)
> - **comments**: 
> - **Tags**: #agent #RAG #KG 

# 🖼 Background & Key Problem

## 1. Existing Agent Memory Paradigm
- The data stored in agent memory is a coherent and highly correlated dialogue stream, rather than independent documents.
- However, most existing systems still follow the standard RAG pipeline:
    - RAG is designed for large, heterogeneous, and diverse corpora
    - It assumes retrieved passages are independent and non-redundant
- When directly applied to agent memory:
    - Similarity-based retrieval tends to return redundant and highly overlapping spans
    - This leads to collapsed retrieval regions and inefficient context usage
- Agent memory retrieval should shift from similarity matching → structured reasoning over latent components
## 2. Limitation of Post-hoc Pruning

- Post-hoc pruning operates on local relevance signals (e.g., token importance, similarity)
- However, in conversational memory:
    - Evidence is often temporally entangled
    - Reasoning relies on multiple dependent pieces (prerequisite chains)
- As a result:
    - Pruning may remove necessary intermediate evidence
    - This breaks the reasoning chain and degrades answer quality
# 🔌 Methodology & Innovation
![[../pic/Pasted image 20260401152140.png]]
## 1. Four-level Hierarchy (Multi-granularity Memory)

The memory is reorganized into four levels:
Original Messages → Episode → Semantic → Theme
- **Episode**: summary of a contiguous block of dialogue
- **Semantic**: distilled long-term reusable facts
- **Theme**: high-level grouping of related semantics

👉 Purpose:
- Disentangle redundant conversational traces
- Preserve intact evidence units
- Enable structured retrieval instead of flat chunk matching

## 2. Guidance Function for Hierarchical Organisation

To maintain a high-quality structure, the paper defines: `f(P) = SparsityScore(P) + SemScore(P)`
### (1) SparsityScore

- Measures how balanced theme sizes are
- Avoids overly large themes (which cause retrieval collapse)
- Effectively controls the branching factor
- More balanced distribution → higher score
### (2) SemScore

`SemScore = (intra-theme similarity) × (inter-theme regularization)`
- Encourages:
    - High intra-theme coherence
    - Moderate inter-theme similarity

Avoids two extremes:
- Redundant themes (too similar)
- Semantic islands (too disconnected)
### (3) Structural Optimization

The function is used to guide:
- **Attach** (assign new semantic nodes)
- **Split** (when a theme is too crowded)
- **Merge** (when themes are too sparse)

👉 Goal: optimize the structure for retrievability rather than pure clustering

## 3. kNN Graph for High-level Navigation

- A kNN graph is maintained over:
    - Theme nodes
    - Semantic nodes
- Each node only keeps top-k similarity edges

👉 Functions:
- Prevent semantic isolation (semantic islands)
- Enable cross-theme traversal
- Support multi-hop reasoning
- Reduce computational complexity (avoid full pairwise comparisons)

👉 Hierarchy provides structure, while kNN graph provides connectivity

## 4. Adaptive Retrieval

Adaptive Retrieval is a **two-stage retrieval framework** that constructs a compact but sufficient evidence set.
### Stage I: Representative Selection (High-level Selection)

- Start with candidate semantic nodes from embedding retrieval
- Instead of selecting top-k by similarity, use a greedy objective: `score = coverage + relevance`
Where:
- **Relevance**: similarity between query and node
- **Coverage**: how many new nodes (via kNN graph) this node can represent

👉 Result:
- Select a small, diverse, and complementary set of semantic nodes
- Avoid redundancy from dense similarity regions
### Stage II: Uncertainty-guided Expansion (Low-level Expansion)

- Start with selected semantics (high-level abstraction)
- Iteratively decide whether to include:
    - Episodes
    - Original messages

Decision rule:
- Include additional evidence only if it reduces model uncertainty

Mechanism:
- Measure whether adding new information improves answer confidence
- Stop expanding when marginal gain is low (early stopping)
### Overall Insight

```
Stage I → decide “what information is needed”  
Stage II → decide “how much detail is needed”
```

# 🔦 Personal Insights

## Strengths

- The method explicitly models the data distribution characteristics of agent memory
- Moves beyond similarity-based retrieval to structure-aware evidence selection
- Combines:
    - Hierarchical abstraction (for compression)
    - Graph connectivity (for composition)
- Effectively balances:
    - Retrieval efficiency
    - Multi-hop reasoning capability

## Limitations

- Dynamic structure maintenance (split/merge + graph updates) may introduce:
    - High computational overhead
    - Latency issues in real-time systems
- The guidance function is hand-crafted, not learned from downstream objectives
- The uncertainty estimation in Stage II may depend on proxy signals, which could be unstable

---
📌 **Personal Recommendation:** ⭐⭐⭐⭐☆