# 🤯Tension Between Retrieval and Reasoning
> Semantic Similarity vs. Logical Similarity
## Dynamic Approaches
### Using CoT to Guide Retrieval
#### [[doc/LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering|LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering]]
+ Start with conventional retrieval, then keep only the retrieved evidence that can support the intermediate steps in the chain of thought; discard the rest.
#### [[short_doc/Interleaving Retrieval with Chain-of-Thought Reasoning for Knowledge-Intensive Multi-Step Questions|Interleaving Retrieval with Chain-of-Thought Reasoning for Knowledge-Intensive Multi-Step Questions]]
+ Begin with standard retrieval, then use the current evidence to generate the next CoT-driven retrieval instruction (a new query), repeating this process until an answer is produced.
### Agent-Based Retrieval
#### [[short_doc/DeepRead Document Structure-Aware Reasoning to Enhance Agentic Search|DeepRead Document Structure-Aware Reasoning to Enhance Agentic Search]]
+ The agent first decides whether retrieval is necessary. If so, it first locates the relevant section and then decides whether deeper reading is needed.
#### [[short_doc/LevelRAG Enhancing Retrieval-Augmented Generation with Multi-hop Logic Planning over Rewriting Augmented Searchers|LevelRAG Enhancing Retrieval-Augmented Generation with Multi-hop Logic Planning over Rewriting Augmented Searchers]]
+ The query is first decomposed into atomic sub-queries. Retrieval is then performed at different granularities depending on the information need, followed by summarization and evaluation before generating the next query.
#### [[short_doc/Multimodal Iterative RAG for Knowledge-Intensive Visual Question Answering|Multimodal Iterative RAG for Knowledge-Intensive Visual Question Answering]]
+ Each retrieval round generates two new queries, one global and one local, to guide the next round until enough evidence is collected to answer the question.

## Static Approaches
### Using a Pre-Built Knowledge Graph to Guide Search
#### [[doc/Knowledge Graph-Guided Retrieval Augmented Generation|Knowledge Graph-Guided Retrieval Augmented Generation]]
+ Build a knowledge graph from the corpus in advance, then augment traditional semantic retrieval with graph-based reasoning to identify additional relevant chunks.
### Building Connections Across Multiple Data Sources
#### [[doc/LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION|LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION]]
+ Extract, deduplicate, explain, and connect entities and relations across the entire corpus in advance to build a unified knowledge graph.
#### [[short_doc/IndexRAG Bridging Facts for Cross-Document Reasoning at Index Time|IndexRAG Bridging Facts for Cross-Document Reasoning at Index Time]]
+ Extract entities from different data sources, merge them into connected subgraphs, and generate new bridging facts at the interfaces between merged subgraphs to support multi-hop retrieval and reasoning.


# 📖Long Context, Large Corpora, and Unstructured Text as Bottlenecks
## Preserving Information at Multiple Granularities
### [[doc/Beyond RAG for Agent Memory Retrieval by Decoupling and Aggregation|Beyond RAG for Agent Memory Retrieval by Decoupling and Aggregation]]
+ Continuous agent memories with high semantic overlap are organized into four levels of granularity: themes, semantics, episodes, and original messages. Retrieval starts from the coarser two levels and only moves to finer-grained information when necessary.
### [[doc/From Local to Global A Graph RAG Approach to Query-Focused Summarization|From Local to Global A Graph RAG Approach to Query-Focused Summarization]]
+ Convert the source text into a graph, then use the `Leiden(G)` algorithm to aggregate subgraphs into a hierarchical structure. Each level is paired with a compressed textual summary so that the LLM can reason over a much broader scope of information.
+ The graph is weighted and undirected, allowing redundancy and repeated information to be reflected naturally in the structure.

## Structuring Text into Paragraph-Level Units
### [[short_doc/DeepRead Document Structure-Aware Reasoning to Enhance Agentic Search|DeepRead Document Structure-Aware Reasoning to Enhance Agentic Search]]
+ First locate the relevant paragraph or section, then retrieve within that structure.

# 🧐Local vs. Global
## Dual-Track Processing
### [[doc/LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering|LongRAG A Dual-Perspective Retrieval-Augmented Generation Paradigm for Long-Context Question Answering]]
+ `Factual Details` preserves the retrieved evidence that supports CoT steps, while all text retrieved through standard RAG is summarized into `Global Information`. Both are used as evidence for answer generation.
### [[short_doc/Multimodal Iterative RAG for Knowledge-Intensive Visual Question Answering|Multimodal Iterative RAG for Knowledge-Intensive Visual Question Answering]]
+ Retrieval is guided by two queries: one based on the global reasoning path and one based on the previous retrieval round. The global query maintains direction, while the local query enables deeper exploration.

# 😶‍🌫️Abstract Questions vs. Concrete Questions
## [[doc/From Local to Global A Graph RAG Approach to Query-Focused Summarization|From Local to Global A Graph RAG Approach to Query-Focused Summarization]]
+ By preserving information at multiple levels of granularity, the system can answer corpus-level, high-level questions using coarse summaries rather than detailed local evidence.
## [[doc/LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION|LIGHTRAG SIMPLE AND FAST RETRIEVAL-AUGMENTED GENERATION]]
+ Entity-based matching and reasoning are better suited to concrete questions, while relation-based matching and reasoning are more suitable for abstract questions.
