# 📜 Basic Information

> - **Link**: [[2505.00212] Which Agent Causes Task Failures and When? On Automated Failure Attribution of LLM Multi-Agent Systems](https://arxiv.org/abs/2505.00212)
> - **Comments**: ICML 2025
> - **Tags**:

# 🖼 Background & Key Problem

## 1. High Cost of Failure Attribution

- Failure attribution in multi-agent systems is labor-intensive and requires domain expertise
- It is currently largely performed manually, making it inefficient and hard to scale
## 2. Bridging Evaluation and Improvement

- The paper emphasizes that:
    > _“Evaluation is not an end in itself, but a means to improvement.”_
- There exists a gap between evaluation results and actionable debugging signals
- The key challenge is to automatically connect evaluation outcomes to specific failure causes

# 🔌 Methodology & Innovation

## 1. Dataset: _Who&When_

- The authors introduce a dataset consisting of failure logs from 127 LLM-based multi-agent systems
- Each sample includes:
    - Failure trajectory (multi-agent interaction logs)
    - The failure-responsible agent (Who)
    - The decisive error step (When)
    - Natural language explanations
- Key characteristics:
    - Covers both:
        - Algorithm-generated systems (automatically constructed and optimized)
        - Hand-crafted systems (manually designed, more realistic)
    - Focuses specifically on failure cases only
    - Annotated by three human experts through multi-round consensus
- Design goal:
    > The dataset is designed to identify _who_ causes the failure and _when_ it occurs within the trajectory.
## 2. Automated Failure Attribution with LLMs

The paper investigates whether LLMs can automatically identify:
- the responsible agent
- the decisive error step
### Key Findings
- **Context size trade-off**:
    - Larger context (all-at-once) → better agent-level accuracy
    - Incremental context (step-by-step) → better step-level accuracy
    > Broad context helps identify _who_,  
    > while localized reasoning helps identify _when_
- **Ground truth helps**:
    - Providing correct answers improves attribution performance
    - Acts as a useful reference signal for detecting deviations
- **Context length matters**:
    - Performance drops as trajectories become longer
    - Step-level attribution is more sensitive to long contexts
- **Method comparison**:
    - Step-by-step performs better when high precision is required
    - All-at-once performs better for global reasoning
- **Statistical vs instance-level performance**:
    - Attribution is unreliable at instance level
    - But becomes meaningful at statistical level (e.g., identifying frequently failing agents)
- **Hybrid methods**:
    - Combining approaches improves performance
    - But introduces higher computational cost
- **Reasoning models**:
    - Stronger reasoning models (e.g., o1, R1) do not consistently outperform
    - Overall performance is still far from practical usability
# 🔦 Summary

- This paper introduces a failure-centric dataset (_Who&When_) for multi-agent systems
- It reframes evaluation into a more actionable problem:  
    👉 identifying _which agent fails and at which step_
- Key insights:
    - Global context → better agent attribution
    - Local reasoning → better step attribution
    - Combining both yields the best results (but still limited)
    - Performance remains far from deployment-ready

---

📌 **Personal Recommendation:** ⭐⭐⭐⭐☆