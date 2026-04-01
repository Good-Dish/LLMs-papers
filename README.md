# LLMs-papers

_A curated collection of reading notes focused on understanding and synthesizing key ideas in Large Language Models (LLMs)._
## 🎯 Purpose

This repository serves as a structured and public record of my academic readings on **LLM-related research**.  
It aims to:
- Summarize and organize the **conceptual understanding** of papers.
- Facilitate clear reflection on **how different research ideas interconnect** across LLM-related subfields.
- Provide an open, well-organized reference that may benefit others exploring similar topics.
## 🧩 About the Notes

Each note provides:
- A structured summary of the paper’s **motivation, methodology, and key contributions**
- Conceptual explanations of the core ideas and reasoning logic
- Personal insights and reflections connecting the paper

> ⚠️ 
> - These notes focus purely on **idea comprehension and reasoning analysis**.  They **do not include any experimental replication or implementation details**.
> - All notes are **language-polished with AI assistance** for readability and academic clarity.  If any expression or description seems unclear, please feel free to open an _Issue_ or leave a comment.

## 🤝 Contribution & Feedback

Feedback, discussion, and suggestions are warmly welcome.  
If you would like to contribute or discuss specific topics, feel free to:
- Submit a **Pull Request** with improved notes or references
- Contact me by **email**

## 📬 Contact

- **Email:** 24171213937@stu.xidian.edu.cn

# 🎞 Paper Index & One-Sentence Summaries

|                                                                                                                  name                                                                                                                  |           tag           |                                                                                                                                            summary                                                                                                                                             |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :---------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|         [[doc/LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering\|LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering]]         | #RAG #CoT #Long-Context |                                                            Introduces a CoT-driven RAG framework where reasoning steps guide the retrieval of relevant chunks, integrating logical reasoning signals into retrieval and generation.                                                            |
|                    [[doc/Plan-on-Graph Self-Correcting Adaptive Planning of Large Language Model on Knowledge Graphs\|Plan-on-Graph Self-Correcting Adaptive Planning of Large Language Model on Knowledge Graphs]]                    |        #RAG #KG         |                                                          Engages LLMs directly in graph exploration, performing stepwise evaluation during KG expansion to enhance reasoning relevance and diversity beyond pure semantic retrieval.                                                           |
|                                 [[doc/Thinking with Knowledge Graphs Enhancing LLM Reasoning Through Structured Data\|Thinking with Knowledge Graphs Enhancing LLM Reasoning Through Structured Data]]                                 |        #RAG #KG         |                                                                                                                       Proposes KGs through programming language syntax.                                                                                                                        |
|                               [[doc/Exploiting Structured Knowledge in Text via Graph-Guided Representation Learning\|Exploiting Structured Knowledge in Text via Graph-Guided Representation Learning]]                               |    #pre-training #KG    |                                                                                       Combines KG-guided masking and contrastive ranking to enhance entity-level relational learning during pretraining.                                                                                       |
| [[doc/MedRAG Enhancing Retrieval-augmented Generation with Knowledge Graph-Elicited Reasoning for Healthcare Copilot\|MedRAG Enhancing Retrieval-augmented Generation with Knowledge Graph-Elicited Reasoning for Healthcare Copilot]] |        #RAG #KG         |                                                  Integrates a diagnostic knowledge graph and EHR retrieval into a reasoning-augmented RAG framework to distinguish diseases with overlapping symptoms and improve clinical interpretability.                                                   |
|                                                       [[doc/KG-RAG Bridging the Gap Between Knowledge and Creativity\|KG-RAG Bridging the Gap Between Knowledge and Creativity]]                                                       |      #RAG #KG<br>       |                                                      Introduces hypernodes to represent relational triples within a multi-level KG and employs a Chain-of-Explorations reasoning process to enhance structured retrieval in RAG systems.                                                       |
|                                                          [[doc/Knowledge Graph-Guided Retrieval Augmented Generation\|Knowledge Graph-Guided Retrieval Augmented Generation]]                                                          |        #RAG #KG         |                                                  Integrates structured KG guidance into semantic retrieval by expanding and filtering multi-hop neighborhoods, enabling LLMs to generate more interpretable and logically consistent answers.                                                  |
|                   [[doc/Everything Everywhere All at Once LLMs can In-Context Learn Multiple Tasks in Super position\|Everything Everywhere All at Once LLMs can In-Context Learn Multiple Tasks in Super position]]                   |     #ICL #mechanism     | This paper shows that LLMs can simultaneously maintain and combine multiple task hypotheses during in-context learning, producing outputs that reflect a weighted mixture of tasks, and provides both empirical evidence and theoretical justification for this task superposition phenomenon. |
|                                                                       [[doc/LLMS GET LOST IN MULTI-TURN CONVERSATION\|LLMS GET LOST IN MULTI-TURN CONVERSATION]]                                                                       |       #mechanism        |                                    This paper shows that LLMs systematically fail in multi-turn conversations with progressively revealed information, and attributes the degradation primarily to increased unreliability rather than reduced capability.                                     |
|                                                                      [[doc/Which Agent Causes Task Failures and When\|Which Agent Causes Task Failures and When]]                                                                      |    #agent #mechanism    |             This paper introduces a failure-centric benchmark (Who&When) and shows that, while LLMs struggle to accurately attribute errors in individual multi-agent trajectories, they can still provide useful insights at a statistical level for diagnosing system failures.              |
|                      [[doc/Re-ranking Reasoning Context with Tree Search Makes Large Vision-Language Models Stronger\|Re-ranking Reasoning Context with Tree Search Makes Large Vision-Language Models Stronger]]                      |    #LVLMs #RAG #ICL     |                    This work formulates context selection for in-context learning as a combinatorial optimization problem and uses MCTS-based search to find the best reasoning context sequence, improving LVLM reasoning performance without modifying model parameters.                     |
|                                                        [[doc/LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION\|LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION]]                                                        |        #RAG #KG         |                  LightRAG enhances retrieval-augmented generation by transforming unstructured text into a graph-structured knowledge representation and performing dual-level retrieval to capture both fine-grained details and global semantic relationships efficiently.                   |
|                                            [[doc/Beyond RAG for Agent Memory Retrieval by Decoupling and Aggregation\|Beyond RAG for Agent Memory Retrieval by Decoupling and Aggregation]]                                            |     #agent #RAG #KG     |        This work proposes a hierarchical and structure-aware retrieval framework for agent memory that replaces similarity-based top-k retrieval with component-level selection and uncertainty-guided expansion to construct compact, non-redundant, and reasoning-complete evidence.         |

