# 📜Basic Information

> - **Link**: [[2509.00798] Multimodal Iterative RAG for Knowledge-Intensive Visual Question Answering](https://arxiv.org/abs/2509.00798)
> - **comments**: 
> - **Tags**: #LVLMs #RAG #reason 
# 🧠 Core Idea

PMSR (Progressive Multimodal Search and Reasoning) addresses a fundamental limitation of traditional Retrieval-Augmented Generation (RAG):
> RAG performs retrieval too early and only once, while reasoning requires progressively refined information.
## Motivation

In knowledge-intensive (especially multimodal) QA:

- The initial query is incomplete and lacks intermediate reasoning signals
- Single-step retrieval often:
  - misses critical evidence
  - retrieves distracting or irrelevant information
- Retrieval and reasoning are decoupled:
  - retrieval cannot benefit from intermediate reasoning
- Iterative/agent-based RAG introduces new issues:
  - long context accumulation
  - error propagation and drift

👉This creates a core dilemma: retrieval needs reasoning, but reasoning depends on retrieval
## Key Insight

PMSR transforms retrieval into a reasoning-driven iterative process:

- Instead of retrieving once → it retrieves progressively
- Instead of raw history → it maintains structured reasoning states
- Instead of static queries → it generates dynamic, state-conditioned queries

# ⚙️ Pipeline
![[../pic/Pasted image 20260407181141.png]]

```
Input: image I + question Q

[Stage 1: Initialization]
1. Generate question-aware image description d0
2. Build initial query qinit = [Q; d0]
3. Retrieve initial evidence D0
4. Summarize into reasoning record r0
5. Initialize trajectory: <r0>

[Stage 2: Iterative Loop]
For t = 1 ... T:
  6. Generate record-level query:
       q_record = [Q; r_(t-1)]

  7. Generate trajectory-level query:
       q_trajectory = Gtrajectory(Q, I, <r0,...,r_(t-1)>)

  8. Perform retrieval using BOTH queries:
       D_record + D_trajectory → Dt

  9. Summarize Dt into new reasoning record rt

 10. Append rt to trajectory

[Stage 3: Termination]
11. If queries become similar (information saturation), stop

[Final Answer]
12. Answer using Q, I, and full trajectory <r0,...,rT>
```

# 🧩 Design Breakdown

## 1️⃣ Iterative Retrieval (solve: incomplete knowledge)

Problem:
- One-shot retrieval cannot cover multi-hop reasoning

Solution:
- Retrieve → update → retrieve again

👉 Enables progressive knowledge acquisition
## 2️⃣ Reasoning Record (solve: error accumulation)

Problem:
- Raw history grows and propagates noise

Solution:
- Compress evidence into structured records

👉 Key effect:
- removes noise
- weakens error propagation
## 3️⃣ Dual-Query Mechanism (solve: search instability)

Problem:
- Single query either:
    - too narrow (error amplification)
    - too broad (slow convergence)

Solution:
- Record-level query → exploitation
- Trajectory-level query → exploration

retrieval = local refinement + global correction

👉 Balances precision and robustness
## 4️⃣ Multimodal Retrieval (solve: grounding mismatch)

Problem:
- Text-only retrieval ignores visual grounding
- Image-only retrieval ignores semantics

Solution:
```
S = λ * sim_text + (1 - λ) * sim_image
```

👉 Ensures:
- semantic relevance
- visual consistency
## 5️⃣ Evidence Aggregation (solve: fragmented reasoning)

Problem:
- Multiple retrieval paths produce disjoint evidence

Solution:
```
Dt = union(D_record, D_trajectory)  
→ LLM reasoning → rt
```

👉 Early fusion (evidence-level), not answer-level

# 📍 Key Design Principles

1. Retrieval is reasoning-guided (not static)
2. State is compressed (reasoning records instead of raw history)
3. Iteration enables multi-hop reasoning
4. Evidence is fused early, not answers
5. Errors are mitigated, not eliminated (there might be some retrieval errors in the middle, but these errors can be compressed and overwritten)

# 🛩️ Final Takeaway

PMSR improves the dynamics of RAG by:
- making retrieval iterative and reasoning-aware
- structuring intermediate states
- reducing error accumulation

In essence: PMSR optimizes how errors evolve, not whether errors exist.
