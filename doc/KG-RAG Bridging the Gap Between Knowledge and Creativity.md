# 📜Basic Information

> - **Link**: [[2405.12035] KG-RAG: Bridging the Gap Between Knowledge and Creativity](https://arxiv.org/abs/2405.12035)
> - **comments**: /
> - **Tags**: #RAG #KG

# 🖼 Background & Key Problem

## 1. **Limitations of Traditional RAG**

- Conventional RAG systems rely primarily on **dense vector similarity search**, which often fails to capture **complex semantic relationships** in multi-hop or creative queries.
- As a result, retrieved knowledge may be **contextually relevant but structurally incomplete**, limiting reasoning accuracy.

# 🔌 Methodology & Innovation

## 1. **Triple Extraction**

- An LLM trained using a **6-shot prompting strategy** extracts as many semantic triples as possible from text chunks, expanding the knowledge base for downstream reasoning.

## 2. **Triple Hypernode Construction**

![[../pic/Pasted image 20251104112522.png]]

- Each **hypernode** represents a single triple and functions as a nested node within the global KG.
- The knowledge graph supports **multi-level nesting**, allowing hierarchical representation of relationships.
- Hypernodes are treated as **standard nodes**, enabling flexible linkage between other hypernodes and ordinary nodes through single relations.

## 3. **Knowledge Graph Retrieval with Chain of Explorations (CoE)**

![[../pic/Pasted image 20251104114356.png]]

- The retrieval process employs the **Chain of Explorations (CoE)** framework, consisting of the following steps:
    1. **Candidate Filtering:** Selects potential nodes based on vector similarity.
    2. **Initial Node Selection:** The LLM identifies the most relevant node among candidates via prompting.
    3. **Relation Ranking:** Relationships connected to the chosen node are ranked by similarity to the query.
    4. **Exploration & Evaluation:**
        - The LLM selects the most relevant relation and traverses to the connected node.
        - If the path is **misdirected**, refinement is performed using a **Chain-of-Thought (CoT)** reasoning process.
        - If the path is **correct but incomplete**, exploration continues along the same route.
        - Once the information is deemed **sufficient**, the model generates the final response.

# 🔦 Personal Insights

## **Strengths**

- The introduction of **hypernodes** provides a compact yet expressive structure for representing **complex relational knowledge** within KGs.

## **Limitations**

- While conceptually elegant, the use of **hypernodes** introduces additional complexity in graph traversal, requiring **carefully designed search algorithms** for efficient retrieval.
- The computational cost of iterative reasoning and multi-level nesting could also limit scalability in large KGs.

---

📌 **Personal Recommendation:** ⭐⭐☆☆☆