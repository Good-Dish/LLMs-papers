# 📜Basic Information

> - **Link**: [[2404.16130] From Local to Global: A Graph RAG Approach to Query-Focused Summarization](https://arxiv.org/abs/2404.16130)
> - **comments**: 
> - **Tags**: #KG #RAG 

# 🖼 Background & Key Problem

## 1. Traditional Vector RAG
- Assumes that answers are contained in small, localized parts of the corpus
- Performs poorly on sensemaking tasks, which require global understanding of the entire dataset
- Such tasks demand aggregation and synthesis over large-scale text, rather than retrieval of isolated facts

# 🔌 Methodology & Innovation

![[../pic/Pasted image 20260408145920.png]]

## 1. Source Documents → Text Chunks
- Larger chunks reduce the number of LLM calls
- However, they may lead to information loss, especially for content appearing early in the chunk
## 2. Text Chunks → Entities & Relationships
- An LLM (via few-shot / in-context learning) extracts:
    - entities
    - relationships
    - descriptions
- Optional: extraction of claims (factual statements)
## 3. Entities & Relationships → Knowledge Graph
- Entities and relationships are aggregated into:
    - nodes (entities)
    - edges (relationships)
- Duplicate occurrences are merged:
    - frequency is used as edge weight
- This step transforms unstructured text into a structured graph representation
## 4. Knowledge Graph → Graph Communities

- Apply Leiden community detection to partition the graph into semantically coherent subgraphs
```
Leiden(G):

    start with every node in its own community

    repeat:
        improve communities by moving nodes locally
        refine each community so it is internally connected
        compress refined communities into supernodes
        continue on the compressed graph
    until no further improvement

    return final community assignment
```
- Result: a hierarchical community structure capturing different levels of semantic granularity
## 5. Graph Communities → Community Summaries

### Leaf-level communities
- Sort edges by descending node degree (importance heuristic)
- Iteratively add:
    - node descriptions
    - edge descriptions
    - related claims
- Stop when reaching the context window limit
### Higher-level communities
- If all element summaries fit within the context window:
    - summarize directly (same as leaf-level)
- Otherwise:
    - replace detailed element summaries with sub-community summaries
    - progressively compress information until it fits
👉 This implements a bottom-up hierarchical summarization mechanism
## 6. Community Summaries → Community Answers → Global Answer
```
Algorithm: Query-Time Global Answering in GraphRAG

Input:
    Q = user query
    S = community summaries at a chosen level
    T_map = token limit for map stage
    T_reduce = token limit for reduce stage
Output:
    A_global = final global answer

Procedure ANSWER_QUERY(Q, S):
    1. Shuffle summaries in S randomly
    2. Partition shuffled summaries into batches such that each batch fits within T_map
    3. For each batch Bi in parallel:
           use LLM to generate:
               Ai = intermediate answer to Q
               hi = helpfulness score in [0, 100]
    4. Filter out all Ai where hi = 0
    5. Sort remaining intermediate answers by helpfulness score in descending order
    6. Iteratively add sorted answers into a new context until token limit T_reduce is reached
    7. Use LLM to synthesize the packed intermediate answers into the final global answer A_global
    8. Return A_global
```
👉 This is essentially a map-reduce style query pipeline:
- Map: local answers from distributed summaries
- Reduce: global synthesis

# 🔑 Key Findings
- Global reasoning ↑ → Directness ↓
- Performance strongly depends on:
    - prompt design
    - extraction quality

# 🔦 Personal Insights

## Strengths
- Transforms raw text into a structured representation (graph)
- Enables global reasoning over the entire corpus, rather than local retrieval
- Improves performance on sensemaking tasks
## Limitations
- Lacks an explicit mechanism to dynamically select relevant communities for a given query
- Current pipeline:
    - processes all communities uniformly
- Potential improvement:
    - introduce “locate → then summarize” strategy
    - instead of aggregating all communities indiscriminately

---
📌 **Personal Recommendation:** ⭐⭐⭐☆☆