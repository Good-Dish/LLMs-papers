# 📜 Basic Information

> - **Link**: [[2505.06120] LLMs Get Lost In Multi-Turn Conversation](https://arxiv.org/abs/2505.06120)
> - **comments**: 
> - **Tags**: #mechanism 

# 🖼 Background & Key Problem

## 1. Limitations of prior multi-turn evaluation

Previous work on multi-turn evaluation often models conversations as episodic sequences, where:
- Each turn is treated as an independent subtask
- Evaluation does not require integrating information across turns

However, this setting diverges from real-world interactions:
- Users frequently begin with underspecified queries and refine them over time
- LLMs tend to make early assumptions and interpret subsequent turns as separate subtasks
- This leads to difficulty in:
    - revising earlier reasoning
    - adapting to newly introduced information

Additionally:
- Some benchmarks decompose multi-turn interactions into predefined subtask types (e.g., refinement, follow-up, expansion)
- This makes it difficult to quantify performance degradation from single-turn to multi-turn settings

👉 **Core gap**: lack of a framework that captures _information accumulation and reasoning correction_ across turns

# 🔌 Methodology & Innovation

## 1. Sharding process & simulation
![[../pic/Pasted image 20260325151726.png]]
- Original fully-specified instructions are decomposed into multiple “shards”, each representing a partial piece of information
- The first shard encodes the high-level intent, while subsequent shards progressively reveal missing details
- Each shard alone is insufficient to solve the task, mimicking real-world underspecification
👉 This simulates progressive information disclosure in natural conversations

Key objective:
- Evaluate whether the model can:
    - revise earlier assumptions
    - incorporate new information
    - recover from incorrect reasoning paths
![[../pic/Pasted image 20260325150448.png]]
Evaluation setup:
- The model generates responses freely at each turn
- The interaction continues until:
    - a correct solution is reached, or
    - all shards are exhausted

## 2. Simulation Types
![[../pic/Pasted image 20260325151815.png]]
The paper defines three simulation settings:
- **FULL**: fully-specified instruction (single-turn upper bound)
- **CONCAT**: all shards concatenated into one prompt (tests robustness to rephrasing)
- **SHARDED**: shards revealed sequentially (true multi-turn setting)

👉 This design isolates:
- reasoning difficulty
- multi-turn interaction difficulty

## 3. Metric Selection

The authors introduce three complementary metrics:
- **Average performance (P)** – expected performance across runs
- **Aptitude (A)** – best-case capability (upper percentile)
- **Unreliability (U)** – variability across stochastic runs

👉 These metrics jointly capture:
- central tendency (mean)
- upper-bound capability
- stochastic instability

This decomposition enables a more fine-grained analysis of performance degradation sources.

## 4. Key Findings

### (1) Universal degradation in multi-turn settings

- All models show performance drops from FULL → SHARDED
- CONCAT shows only minor degradation  
    → indicating that rephrasing alone is not the issue

👉 The core difficulty lies in multi-turn reasoning, not input format

### (2) Model size does not solve the problem

- Both small and large models experience ~30–40% degradation in SHARDED
- Smaller models (8–13B) additionally show weaker generalization

### (3) Degradation is task-dependent

- Performance drop varies across domains
- Tasks with episodic structure (e.g., translation) are less affected

👉 Suggests: multi-turn failure is closely tied to cross-turn dependency and reasoning continuity

### (4) Test-time compute is insufficient

- Increasing reasoning tokens does not significantly improve performance

👉 Models struggle with strategy over turns, not just computation per turn

### (5) Reliability is the main bottleneck

- In SHARDED:
    - both aptitude decreases
    - and unreliability increases
- However, performance degradation is primarily driven by: increased unreliability (variance), not just loss of capability

### (6) Temperature reduction is ineffective

- Lowering temperature does not significantly improve reliability

Reason:
- Single-turn → limited deviation
- Multi-turn → error accumulation across turns

### (7) Core conclusion

Any multi-turn interaction involving underspecification leads to models “getting lost in conversation.”

# 🔦 Personal Insights

This work is highly **systematic, rigorous, and well-structured**.

From my perspective: **simulation design** and **metric decomposition** are the most critical contributions

> 👉 The paper provides a strong example of: transforming an abstract failure mode into a **measurable experimental framework**
    

Additionally:
- The authors go beyond empirical observation by analyzing  which factors (aptitude vs. reliability) drive performance differences
- This makes the conclusions more diagnostic rather than purely descriptive

---

📌 **Personal Recommendation:** ⭐⭐⭐⭐⭐