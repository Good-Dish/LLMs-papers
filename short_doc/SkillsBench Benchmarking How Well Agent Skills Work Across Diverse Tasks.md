# 📜Basic Information

> - **Link**: [[2602.12670] SkillsBench: Benchmarking How Well Agent Skills Work Across Diverse Tasks](https://arxiv.org/abs/2602.12670)
> - **comments**: 
> - **Tags**: #agent #skills 
> - **Associated with**: [[../doc/SWE-Skills-Bench Do Agent Skills Actually Help in Real-World Software Engineering|SWE-Skills-Bench Do Agent Skills Actually Help in Real-World Software Engineering]]


# 🧮Research Methodology

## Core Idea

- Evaluate Agent Skills (procedural knowledge) as a *separate, controllable variable*
- Move beyond “model capability” → measure **augmentation effectiveness**
👉 from evaluating models to evaluating what improves models
## Experimental Design

Three controlled conditions:
- No Skills → baseline capability  
- Curated Skills → human-designed procedural knowledge  
- Self-generated Skills → model-generated knowledge  
👉 Same task + same model → isolate the effect of Skills
## Benchmark (SkillsBench)

Each task includes:
- Instruction → task description  
- Environment → Docker setup (data + tools + Skills)  
- Solution → oracle (guaranteed correct)  
- Verifier → deterministic tests (pass/fail)  
👉 No LLM judge → fully **reproducible and objective**
## Scale

- 84 tasks  
- 11 domains  
- 7 model–agent configs  
- 7308 trajectories  
👉 Large enough for meaningful comparisons
## Metrics

- Pass Rate → success rate  
- Normalized Gain → relative improvement  
👉 Captures both absolute and proportional gains

# 🗞️Task Selection Principles

## Selection Pipeline

- 322 candidate tasks (community)
- Automated validation
- Human review
- Final: 84 tasks
👉 Strict filtering ensures quality and realism
## Human Review Criteria

Tasks must satisfy:
- Real data (not toy)
- Real workflows
- Valid oracle solution
- High-quality Skills
- No cheating shortcuts
👉 Reviewers also test tasks with/without Skills
## Core Design Principles

- Skill-dependent: tasks solvable without Skills, but easier with them  
- Deterministic evaluation: programmatic verification only  
- No leakage: skills cannot contain answers  
- Realistic: reflect real-world workflows  
- Compositional: tasks require multiple Skills (3–6)
👉 Benchmark is designed to stress procedural knowledge
## Task Distribution

- 11 domains (SE, healthcare, finance, etc.)
- Difficulty based on human time:
  - Core (<1h)
  - Extended (1–4h)
  - Extreme (>4h)
👉 Practical difficulty definition

# 📍Key Findings

## Skills Help, but Not Always

- Average improvement: +16.2pp
- High variance across tasks
👉 Skills are context-dependent, not universally helpful
## Self-generated Skills Fail

- No improvement or negative impact
👉 LLMs cannot reliably generate usable procedural knowledge
## Domain Matters

- Large gains: healthcare, manufacturing  
- Small gains: math, software engineering  
👉 Gains correlate with procedural knowledge gaps
## Skill Design Matters

- 2–3 Skills → best performance  
- Too many → conflict and overload  
- Concise > long  
- Step-by-step > descriptive  
👉 Good Skills = focused, actionable, minimal
## Skills vs Model Size

- Small model + Skills ≈ large model (no Skills)
👉 Skills act as a capability multiplier
## Negative Effects Exist

- ~20% tasks show performance drop
Reasons:
- conflicting guidance  
- unnecessary complexity  
- already-solved tasks  
👉 Skills can introduce noise
## Core Insight

👉 Skills close the procedural gap
- Models know *what*  
- Skills provide *how*

---

## 4. One-line Takeaway

👉 High-quality procedural knowledge can significantly boost agent performance, but its effectiveness depends on task, design, and system integration.