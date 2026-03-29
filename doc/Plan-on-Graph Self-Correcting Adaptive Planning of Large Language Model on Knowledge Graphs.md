# 📜Basic Information

> - **Link**: [[2410.23875] Plan-on-Graph: Self-Correcting Adaptive Planning of Large Language Model on Knowledge Graphs](https://arxiv.org/abs/2410.23875)
> - **comments**: /
> - **Tags**: #RAG #KG

# 🖼 Background & Key Problem

## 1. **Hallucination in LLMs**

- Incorporating external knowledge from knowledge graphs (KGs) extends LLMs beyond their parameter-based knowledge, but also increases the risk of **hallucination** — fabricating answers when the question lies outside the model’s internal domain.
- Pretraining or fine-tuning can inject new knowledge into model parameters, yet these methods still suffer from **inflexibility**, **unreliability**, and **limited interpretability**.

## 2. **Limitations of KG-Augmented LLMs**

- The effectiveness of some approaches depends heavily on the completeness of KGs: they extract answer-related triples and pass them to the LLM, but the model itself does not actively participate in KG search.
- Although KGs provide structured knowledge, current KG-augmented paradigms often fail in tasks that require complex reasoning.
- Many frameworks **predefine a fixed KG search range**, but correct answers may lie beyond it.
- KG search is typically **directional and irreversible** — if the traversal direction is wrong, there are no effective correction mechanisms.
- Important contextual information from the original question can be lost during graph traversal, leading to reasoning errors.
![[../pic/stitching-collage-1761274637626.png]]

# 🔌 Methodology & Innovation

![[../pic/Pasted image 20251024105146.png]]

## 1. **Task Decomposition**

- The LLM decomposes a complex question into several sub-goals, forming the foundation for adaptive reasoning.

## 2. **Path Exploration**

- The initial exploration points are defined by the **intersection between the annotated dataset and the sub-goals**.
- **Relationship Search:**
    - The candidate relationship set consists of all relations connected to entities from the previous path step.
    - The LLM dynamically selects the most relevant relationships for each sub-goal, with no fixed limit on the number of relations.
- **Entity Search:**
    - Candidate entities are connected via existing graph edges.
    - **DistilBERT** filters entities based on question relevance.
    - The LLM then refines this set to extract the most relevant entities for the current search round.

## 3. **Memory Updating**

- **Subgraph:** the set of all visited entities and relationships.
- **Reasoning Paths:** currently valid traversal paths maintained by the algorithm.
- **Sub-Objective Status:** tracks each sub-goal’s progress, recording what is known, missing, or completed.

## 4. **Evaluation & Self-Correction**

- The LLM decides whether sufficient knowledge has been gathered to generate an answer.
    - **If yes:** produce the final answer.
    - **If no:**
        - Expand current paths further if information is incomplete.
        - Re-evaluate directionality — assessing each entity and relation in the reasoning path for possible redirection.

# 🔦 Personal Insights

## **Strengths**

- Involving the LLM directly in KG search improves **knowledge acquisition and reasoning flexibility**.
    - The model relies not only on semantic similarity but also on **structured reasoning**.
    - The retrieved information remains more **contextually relevant** to the question.

## **Limitations**

- **High computational cost:**
    - The LLM is engaged in every reasoning step.
    - For long reasoning paths or large graphs, the process becomes **time-intensive** and may hinder scalability.
	- Future work could explore lightweight reasoning modules or hybrid symbolic–neural retrieval to mitigate the time overhead.

---

📌 **Personal Recommendation:** ⭐⭐⭐☆ ☆