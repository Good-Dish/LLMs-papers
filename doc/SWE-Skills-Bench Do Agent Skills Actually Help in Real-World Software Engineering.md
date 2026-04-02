# 📜Basic Information

> - **Link**: [[2603.15401] SWE-Skills-Bench: Do Agent Skills Actually Help in Real-World Software Engineering?](https://arxiv.org/abs/2603.15401)
> - **comments**: 
> - **Tags**: #agent #skills

# 🖼 Background & Key Problem

## 1. Agent Skills

- Agent skills are structured procedural knowledge packages, typically implemented as markdown files (SOP-style guidance).
- They encode:
    - standard operating procedures
    - reusable templates
    - domain conventions
- Skills are injected into the prompt at inference time, acting as behavioral guidance rather than knowledge retrieval.
- Skill = behavioral control over reasoning process
## 2. Software Engineering (SWE)

- SWE tasks exhibit several fundamental properties:
	- **Requirement-driven**: success is defined by satisfying all acceptance criteria
	- **Executable verification**: correctness is determined by running code (unit tests), not subjective judgment
	- **Multi-step & long-horizon**: tasks involve iterative processes: coding → testing → debugging
	- **Constraint-heavy**: must obey APIs, frameworks, compatibility rules
	- **Context-sensitive**: strong dependence on repository and environment
- SWE is not free-form generation, but a constrained, requirement-driven optimization problem
## 3. Limitations of Existing Benchmarks

- **SkillsBench** (prior work):
    - First benchmark for agent skills
    - BUT:
        - SWE only accounts for a small portion (16/84 tasks)
        - Focuses on cross-domain skill efficacy
        - Does NOT evaluate requirement satisfaction in real workflows
- Existing benchmarks do not measure whether skills actually help complete real SWE tasks under strict requirements.
# 🔌 Methodology & Core Design
![[../pic/Pasted image 20260402141811.png]]
## 1. Benchmark Pipeline
### (1) Skill Curation

- Goal: construct a deterministic and testable subset of skills
- Steps:
	1. Select SWE-relevant categories from large skill ecosystem
	2. Filter for concrete, actionable skills (exclude vague/generative ones)
	3. Remove skills with high environment/setup cost
- Result: 49 curated SWE skills
### (2) Task Instance Generation

- Goal: embed each skill into real-world development scenarios
- For each skill, construct ~10 task instances
- Process:
	1. **Project Matching**
	    - Select real GitHub repositories
	    - Ensure tech stack aligns with skill domain
	2. **Requirement Authoring**
	    - Generate structured requirement document:
	        - Background
	        - Task description
	        - File operations
	        - Acceptance criteria
	    - Requirements must:
	        - be repository-specific
	        - trigger relevant skill usage
	        - NOT leak skill content
	3. **Skill Placement**
	    - Skill is injected at environment level
	    - Agent automatically detects and uses it
### (3) Requirement-driven Verification

- Goal: ensure objective and deterministic evaluation
- Core idea: convert acceptance criteria → executable tests
- Design principles:
	- **Discriminative Power**
	    - Distinguish real success vs superficial outputs
	- **Behavioral Verification**
	    - Execute code, not just check surface patterns
	- **Completeness**
	    - Cover all acceptance criteria
	- **Non-trivial Assertions**
	    - Validate correctness of logic and outputs
## 2. Evaluation Setup

- Paired evaluation:
    - with skill vs without skill
- Same task, same environment
- Only variable: skill injection
# 📊 Key Findings
## 1. Most Skills Provide No Benefit

- 39 / 49 skills → ΔP = 0
- ~80% of skills have **no effect**
Two cases:
- Model already solves task → skill redundant
- Model fails → skill insufficient
👉 **Skill is NOT a universal performance booster
## 2. Token Cost ≠ Performance

- Token overhead ranges widely (-77% to +450%)
- No correlation with accuracy
👉 **Skills change reasoning paths, not necessarily outcomes**

## 3. A Small Subset of Skills Works

- Only 7 skills show improvement (+7% ~ +30%)
- Characteristics:
	- Domain-specific
	- Provide missing knowledge
	- Cover model blind spots
👉 **Skill is useful only when it fills a capability gap**

## 4. Skills Can Hurt Performance (Critical)

- Some skills cause **negative ΔP**
- Root cause: **context interference**
- Mechanisms:
	- **Surface Anchoring**
	    - Model copies incorrect template details
	- **Hallucination**
	    - Conflicting info → fabricated structures
	- **Concept Bleed**
	    - Mixing unrelated concepts
👉 **Incorrect or overly specific skills can degrade reasoning**

---
📌 **Personal Recommendation:** ⭐⭐⭐☆ ☆