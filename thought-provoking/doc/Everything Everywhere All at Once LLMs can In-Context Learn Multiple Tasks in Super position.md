# 📜 Basic Information

- **Link**: [Everything Everywhere All at Once: LLMs can In-Context Learn Multiple Tasks in Superposition | OpenReview](https://openreview.net/forum?id=98NalIH588)
- **Comments**: ICML 2025 Spotlight Poster
- **Tags**:
# 🖼 Background & Key Problem

## 1. In-Context Learning (ICL)
- ICL enables LLMs to perform a wide range of tasks without fine-tuning, simply by conditioning on a few examples in the prompt.
- However, the underlying mechanism of how ICL works remains largely unclear.
## 2. Task Superposition
- This paper introduces the concept of task superposition, where LLMs can perform multiple distinct ICL tasks within a single inference pass.
- Instead of committing to a single task, the model maintains a distribution over multiple task hypotheses.

# 🔌 Key Findings

## 1. LLMs can perform multiple tasks in superposition under mixed-task prompts
- When the prompt contains examples from multiple tasks, LLMs assign non-negligible probabilities to the correct outputs of multiple tasks simultaneously.
- Different models exhibit different task preferences under the same prompt.
- Larger models within the same family:
    - Handle more tasks in parallel
    - Better calibrate output probabilities with respect to the task distribution in the prompt
## 2. Superposition emerges even with single-task training
- The authors design synthetic tasks and train a small model using only single-task prompts.
- During inference with mixed-task prompts, the model:
    - Maintains multiple task hypotheses simultaneously
    - Allocates probabilities to different task outputs
    - Shows an approximately linear relationship between task proportions in the prompt and output probabilities

👉 This suggests that task superposition is an inherent capability of the ICL mechanism, rather than a byproduct of multi-task training.
## 3. Task superposition is linked to internal task vector composition
- The model internally represents tasks using task vectors, rather than explicit task selection.
- A task vector is defined (operationally) as:
    - task vector ≠ prompt embedding
    - task vector ≈ (h_{task} - h_{baseline})
Where:
- (h_{task}): hidden representation under a task-specific prompt
- (h_{baseline}): hidden representation under a non-task prompt
- During superposition, these task vectors are combined (approximately linearly) in the representation space.

# 🔦 Personal Insights

## Strengths
- The paper identifies and clearly demonstrates a previously underexplored phenomenon:  LLMs can implicitly perform multiple tasks within a single inference.
- It combines:
    - empirical evidence (experiments)
    - theoretical justification (Transformer expressivity via constructive proof)
- The task vector perspective provides a plausible mechanistic explanation for superposition.
## Limitations
- The theoretical result is an existence proof based on a simplified Transformer construction, which may not directly reflect real LLM behavior.
- Empirical analysis is somewhat limited:
    - More diverse LLM families
    - More realistic task settings  
        could strengthen the conclusions.
- The task vector explanation, while suggestive, is not yet a complete mechanistic account.

---
📌 **Personal Recommendation:** ⭐⭐⭐☆ ☆