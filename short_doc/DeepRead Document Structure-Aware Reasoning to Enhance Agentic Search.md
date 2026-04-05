# 📜Basic Information

> - **Link**: [[2602.05014] DeepRead: Document Structure-Aware Reasoning to Enhance Agentic Search](https://arxiv.org/abs/2602.05014)
> - **comments**: 
> - **Tags**: #agent #RAG 

# 🧠 Core Idea

> DeepRead is designed to solve a core weakness of standard RAG on long documents: **structural blindness**.

In conventional RAG, documents are usually split into flat chunks and retrieved as isolated pieces. This often causes three problems:
- relevant evidence is fragmented across chunks
- the system must guess many keywords to retrieve all needed pieces
- the model sees snippets without enough local structure or reading order

To address this, DeepRead shifts Retrieval-Augmented Generation (RAG) from **“retrieve answers”** to **“locate then read”**.

Instead of treating documents as flat chunks, it leverages document structure (sections and paragraphs) to enable a more human-like reasoning process:
- First, **locate a relevant region**
- Then, **read it continuously**
- Finally, **reason and answer**

# ⚙️ Pipeline

```text
Raw Documents
    ↓
Structure Parsing
    - OCR / parser converts documents into structured text
    - recover headings, paragraphs, and reading order
    ↓
Document Representation
    - build Table of Contents (TOC)
    - assign each paragraph a coordinate:
      (doc_id, sec_id, para_idx)
    ↓
Agent Initialization
    - input to agent:
      1. user question
      2. TOC / document schema
    ↓
Agent Loop

while not finished:
    1. agent decides whether to call Retrieve
    2. Retrieve(query):
         - semantic retrieval over paragraphs
         - return paragraph text + coordinates
    3. agent reads retrieval results
    4. agent decides whether to call ReadSection
    5. ReadSection(doc_id, sec_id, start, end):
         - return contiguous paragraphs from a section
         - preserve original order
    6. returned text is appended into context
    7. agent reasons over accumulated evidence
    8. agent decides:
         - retrieve again
         - read more
         - or answer
    ↓
Final Answer
```

# 🔍 Retrieval Mechanism

- Retrieval is still **embedding-based semantic search**
- The unit of retrieval is:
  → **paragraph (not fixed-size chunk)**

Each retrieved result includes a coordinate: `(doc_id, sec_id, para_idx)`
This acts as a **navigation anchor**, not the final answer.

# 📖 Reading Mechanism

After retrieval, the system does not directly answer.
Instead, it performs `ReadSection(doc_id, sec_id, start, end)`

This:
- Fetches **contiguous paragraphs**
- Preserves **order and structure**
- Injects raw text directly into LLM context

Key idea:
→ Retrieval gives a *point*, reading gives a *region*

# 🔁 Agent Loop

The system follows a ReAct-style loop:
```text
Question
 → Retrieve (find anchor)
 → ReadSection (expand context)
 → Reason
 → Decide next step:
      - Retrieve again
      - Read more
      - Answer
```

Important:
- The **LLM controls the process**
- Retrieved text is **not reused as query directly**
- Instead, it **influences the next query via reasoning**

# 💡 Key Insights

- Retrieval ≠ answer  
- Retrieval = **navigation**

DeepRead shifts from:

- Traditional RAG:
  → retrieve → generate

To:

- DeepRead:
  → locate → read → reason

# ⚖️ Strengths

- Handles long documents better
- Reduces context fragmentation
- Requires only partial recall (find one anchor instead of all evidence)
- Mimics human reading behavior

# ⚠️ Limitations

## 1. Dependence on Document Structure

DeepRead assumes:
- Documents have **recoverable structure**
- (headings, paragraphs, hierarchy)

If input is:
- raw text
- poorly formatted OCR
- no clear sections

Then:
- structure becomes unreliable
- ReadSection loses effectiveness

## 2. Weak for Structure-less Text

When documents lack structure:

- Must artificially segment text
- This becomes **structure induction**, not recovery
- May degrade performance

## 3. Retrieval Precision for Small Answers

For questions with:
- very short answers (numbers, names)
- weak contextual signals

Embedding retrieval may:
- fail to hit the correct paragraph
- fail to locate the correct region

In such cases:
- traditional RAG (exact match) may outperform

## 4. Higher Computational Cost

Compared to standard RAG:

- More tool calls (Retrieve + Read)
- More tokens consumed (reading sections)

Trade-off:
→ higher cost for higher accuracy

## 5. Retrieval Still the Bottleneck

DeepRead still relies on:
- initial semantic retrieval

If retrieval fails to find a good anchor:
- the whole pipeline fails

# 🧠 Final Summary

DeepRead transforms RAG from:

→ **“finding answers”**

into:

→ **“navigating and reading documents”**

It works best when:
- documents are structured
- answers require context
- reasoning spans multiple parts

But struggles when:
- structure is missing
- answers are extremely localized
