
# 📜 Basic Information

> - **Link**: [[2506.07785] Re-ranking Reasoning Context with Tree Search Makes Large Vision-Language Models Stronger](https://arxiv.org/abs/2506.07785)
> - **comments**: ICML 2025 spotlight
> - **Tags**: #LVLMs #RAG #ICL 
# 🖼 Background & Key Problem

## 1. Factual Inconsistencies
- Retrieval-Augmented Generation (RAG) helps improve factual correctness by grounding model outputs in external knowledge.
## 2. Instruction Misalignment
- Existing RAG and in-context learning (ICL) methods provide limited guidance on _how to reason_ using retrieved knowledge.
- Retrieved samples are often:
    - Presented in rigid formats (e.g., direct answers), which limits the model’s ability to learn underlying reasoning patterns.
    - Inconsistent in quality, meaning that retrieved examples do not always lead to improved performance.

# 🔌 Methodology & Innovation
![[../pic/Pasted image 20260329171617.png]]
## 1. Reasoning Context with Self-Consistent Evaluation
![[../pic/Pasted image 20260329171933.png]]
- Existing knowledge bases typically lack explicit reasoning processes.
- The paper augments each question–answer pair with reasoning context:
    - Generate multiple candidate reasoning paths using LVLMs.
    - Feed each candidate back into the model to predict answers.
    - Score predictions and select the reasoning context associated with the highest-scoring answers.
- This process ensures that stored contexts contain reliable reasoning patterns, not just final answers.
## 2. Knowledge Retrieval with Hybrid Embeddings
- Both textual and visual features are encoded and concatenated into a hybrid embedding.
- Queries and database entries are encoded using the same encoder.
- Retrieval is performed based on similarity (distance) in the hybrid embedding space.
- This enables more accurate alignment between multimodal queries and knowledge base entries.
## 3. Re-ranking by Tree Search with Heuristic Rewards
![[../pic/Pasted image 20260329173858.png]]
- Each retrieved sample is treated as a context unit, consisting of: image, question, answer, and reasoning context.
- The goal is to find the optimal sequence of contexts for in-context learning.
### Node Expansion
- Each node represents a context sequence.
- Expansion is performed by appending a new context to the current sequence: if adding a context improves performance, the sequence is further explored.
### Scoring Mechanism
Each context sequence is evaluated using a hybrid reward:
- Accuracy (when ground truth is available)
- Heuristic scoring (core in inference)
    - Based on self-consistency evaluation:
        - Construct a prompt using the current context sequence.
        - Generate answers multiple times.
        - Measure the stability and agreement of outputs.
# 🔦 Personal Insights
## Strengths

- Proposes a novel framework that formulates context selection as a combinatorial optimization problem.
- Introduces MCTS to explicitly model interactions between context examples, rather than treating them independently.
- Enhances ICL by incorporating reasoning-aware contexts, improving reasoning quality.
## Limitations

### 1. High Computational Cost
- Tree search introduces significant overhead:
    - Although more efficient than exhaustive search, it still requires multiple forward passes per query.
    - Not suitable for latency-sensitive applications.
### 2. Lack of Reliable Evaluation Signal in Inference
- In real-world scenarios, ground truth is unavailable.
- The method relies on heuristic scoring:
    - Optimizes a proxy objective (self-consistency) rather than true correctness.
    - May produce confident but incorrect answers.
        

---
📌 **Personal Recommendation:** ⭐⭐⭐☆ ☆