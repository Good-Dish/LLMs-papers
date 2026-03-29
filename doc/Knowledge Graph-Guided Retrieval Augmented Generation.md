# 📜Basic Information

> - **Link**: [[2502.06864] Knowledge Graph-Guided Retrieval Augmented Generation](https://arxiv.org/abs/2502.06864)
> - **comments**: 2025 Annual Conference of the Nations of the Americas Chapter of the ACL (NAACL 2025)
> - **Tags**: #RAG #KG

# 🖼Background & Key problem

## 1. [[KG-RAG Bridging the Gap Between Knowledge and Creativity# 🖼 Background & Key Problem|Background_1]]
## 2. [[Plan-on-Graph Self-Correcting Adaptive Planning of Large Language Model on Knowledge Graphs# 🖼Background & Key problem|Background_2]]

# 🔌 Methodology & Innovation

![[../pic/Pasted image 20251108141746.png]]

## 1. **Knowledge Graph Construction and Connection**

- When an existing KG is available, **entity recognition and linking algorithms** are applied to connect text chunks with corresponding entities and relations.
- Alternatively, a **sub-KG** can be constructed within each text chunk and later clustered into an integrated KG.
- Each knowledge entry is represented as a **quadruple**:
    - _head entity_
    - _relation_
    - _tail entity_
    - _source chunk_ (the textual origin of the triple)

## 2. **Semantic-Based Retrieval**

- A conventional retrieval step retrieves text chunks based on **semantic similarity** to the query.

## 3. **Retrieval Extension via KG Guidance**

- Entities and relations mentioned in the retrieved chunks are identified.
- Their **M-hop neighborhoods** are selected as candidate expansions within the KG.
- To refine these expansions:
    - An **undirected weighted graph** is constructed based on semantic similarity.
    - The graph is divided into connected components, and for each component, a **Maximum Spanning Tree (MST)** is generated to retain only the most relevant structure.
    - Redundant edges are removed, and the system performs **depth-first traversal** beginning from the edge with the highest weight to select relevant relations and nodes, and their corresponding chunks are regarded as **text representations**.
- Corresponding **text-form paragraphs** (converted from each MST triples) are scored for relevance and ranked. The top-_k_ paragraphs form the **triple representations**.

## 4. **Answer Generation**

- The LLM integrates three sources of external knowledge:
    1. Retrieved text chunks
    2. Text representations
    3. Triple representations
- The integrated knowledge is then used to generate a final answer with improved reasoning coherence.

# 🔦 Personal Insights

## **Strengths**

- The explicit use of **knowledge graphs to guide retrieval** enhances interpretability and logical consistency compared with standard semantic-only RAG systems.
- The inclusion of both **triples** and their **derived text chunks** provides a bridge between structured and unstructured information, making the retrieved context more comprehensible to LLMs.

## **Limitations**

- Combining entities across disconnected subgraphs remains difficult due to inconsistent or overlapping textual representations, which may result in **redundant or fragmented graph structures**.
- The method is **computationally intensive**, as nearly every stage requires searching and scoring operations.
- The model’s performance is probably **sensitive to the top-_k_ parameter**:
    - A high _k_ increases processing load and noise;
    - A low _k_ risks losing relevant yet diverse information.

---

📌 **Personal Recommendation:** ⭐⭐⭐☆ ☆