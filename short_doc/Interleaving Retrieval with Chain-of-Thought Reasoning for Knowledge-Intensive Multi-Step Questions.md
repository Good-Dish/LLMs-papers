# 📜Basic Information

> - **Link**: [[2212.10509] Interleaving Retrieval with Chain-of-Thought Reasoning for Knowledge-Intensive Multi-Step Questions](https://arxiv.org/abs/2212.10509)
> - **comments**: 
> - **Tags**: #CoT #RAG 

# 🧠 Core Idea

IRCoT (Interleaving Retrieval with Chain-of-Thought) addresses a fundamental limitation of traditional Retrieval-Augmented Generation (RAG):
> **Static, one-shot retrieval fails for multi-hop reasoning tasks.**

**Motivation:**

Traditional RAG assumes: 
- A single query is sufficient to retrieve all relevant knowledge 
- Retrieval and reasoning are independent

However, in multi-step QA: 
- Required knowledge is **not fully specified in the original query** 
- Retrieval depends on **intermediate reasoning results** 
- Missing intermediate entities leads to retrieval failure

Example pattern: question → intermediate entity → new retrieval → answer

👉 Therefore: **retrieval should depend on reasoning state, not just the
initial query**

# 🔄 Pipeline

IRCoT transforms RAG into an iterative loop:
```
    Input: Question Q

    Step 1: Retrieve initial documents using Q

    Loop:
        Step 2: Generate next CoT sentence (based on Q + retrieved docs + previous CoT)
        Step 3: Use last CoT sentence as new query
        Step 4: Retrieve new documents
        Step 5: Accumulate documents

    Stop when:
        - "answer is" appears
        - max steps reached

    Final:
        Use all retrieved documents → QA Reader → Answer
```

Key properties: - Retrieval is **multi-step and dynamic** - Query
evolves via reasoning - Context is **accumulated (not replaced)**

# ⚙️ Key Design Choices

-   **Retriever**: BM25 (no training required)
-   **Query reformulation**: last CoT sentence
-   **CoT control**: only first generated sentence per step
-   **Retrieval budget**: small K per step, capped total
-   **Accumulation**: all retrieved passages kept

# ⛓️ Retrieval vs QA Decoupling

Even though IRCoT generates CoT during retrieval:

👉 It **still uses a separate QA reader**

Why?

-   Retrieval CoT = **search policy**
-   QA reader = **final reasoning over full evidence**

Using CoT directly for answers is worse because:
- It is generated incrementally (local view) 
- It is constrained (single-sentence steps) 
- It is not optimized for final answer quality

# 🤖 Reader Design

Two reader strategies:
### 1. CoT Prompting (GPT-style)
-   Generates reasoning + answer
-   Better for strong reasoning LLMs (e.g., GPT3)
### 2. Direct Prompting (T5-style)
-   Outputs answer directly
-   More stable for smaller / weaker CoT models

👉 **Not all models benefit from explicit reasoning at answer time**

# ⚠️ Limitations

## ❗ From the paper
-   Requires strong CoT capability in LLMs
-   Needs long context (multi-step + demos + docs)
-   High computational cost (multiple LLM calls)
-   Fixed reasoning--retrieval schedule (no adaptive policy)
-   Partial reliance on external APIs
## ❗ Deeper limitations

### 1. Error propagation from CoT
-   Incorrect intermediate reasoning → bad retrieval
-   Errors compound across steps
### 2. Greedy retrieval (no global planning)
-   Each step is locally optimal
-   No multi-path exploration
### 3. No structured reasoning state
-   Uses free-form text instead of graphs/entities
-   Hard to control and debug
### 4. Append-only memory
-   Retrieved noise cannot be removed
-   Early mistakes persist in context
### 5. Weak coupling between retrieval and reader
-   Reader does not refine retrieval
-   No feedback loop
### 6. Scalability issues
-   Long context + multi-step calls = expensive
-   Hard to deploy in real-time systems

# 💡 Key Insight

IRCoT reframes retrieval as:

👉 **A reasoning-guided, iterative process instead of a static lookup**

