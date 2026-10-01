# RAG — Summary

This file is a quick revision guide for the complete RAG section.

Use it before an interview to quickly revise the important concepts, terminology, architecture, and troubleshooting approaches.

---

# 1. RAG Fundamentals

## What is RAG?

**Retrieval-Augmented Generation (RAG)** combines information retrieval with an LLM.

Instead of relying only on the LLM's internal knowledge, RAG retrieves relevant external information and provides it to the LLM as context.

```text
User Query
    ↓
Retrieval
    ↓
Relevant Documents
    ↓
Context
    ↓
LLM
    ↓
Answer
```

## Why use RAG?

RAG is useful for:

- Private / enterprise data
- Domain-specific knowledge
- Frequently changing information
- Large document collections
- Knowledge bases

## RAG vs Fine-tuning

| RAG | Fine-tuning |
|---|---|
| Adds external knowledge at query time | Changes model behavior/weights |
| Good for changing knowledge | Better for behavior/style/task adaptation |
| Easier to update knowledge | Updating knowledge requires retraining/further training |
| Provides source context | Knowledge is incorporated into model parameters |

## RAG vs Long Context

Long-context prompting puts a large amount of information directly into the prompt.

RAG first retrieves the relevant information and sends only selected context to the LLM.

---

# 2. Document Ingestion & Preprocessing

## Document Ingestion

Document ingestion is the process of:

```text
Documents
   ↓
Text Extraction
   ↓
Cleaning
   ↓
Chunking
   ↓
Embedding
   ↓
Vector Database
```

## Why Cleaning?

Cleaning removes unnecessary content such as:

- Formatting noise
- Duplicate text
- Headers/footers
- Irrelevant content
- Extraction errors

## What is Chunking?

Chunking divides large documents into smaller pieces that can be independently retrieved.

```text
Large Document
      ↓
 ┌────┬────┬────┬────┐
 │ C1 │ C2 │ C3 │ C4 │
 └────┴────┴────┴────┘
```

## Chunk Size

- Too small → insufficient context
- Too large → irrelevant information and more tokens

## Chunk Overlap

Overlap preserves information across chunk boundaries.

```text
Chunk 1: A B C D E
Chunk 2:       D E F G H
                   ↑
              Overlap
```

## Chunking Strategies

- Fixed-size chunking
- Recursive chunking
- Semantic chunking
- Document-structure-based chunking

Choose the strategy based on the structure and characteristics of the dataset.

---

# 3. Embeddings

## What is an Embedding?

An embedding converts text into a numerical vector representing its semantic meaning.

```text
"How does RAG work?"
          ↓
     Embedding Model
          ↓
[0.12, -0.43, 0.76, ...]
```

## Why Embeddings?

They allow the system to compare the semantic similarity between queries and documents.

```text
Query
  ↓
Embedding
  ↓
Vector Similarity
  ↓
Similar Documents
```

## Embedding Dimensionality

Dimensionality is the number of values in an embedding vector.

Example:

```text
[0.2, 0.5, -0.1, 0.8]
```

has 4 dimensions.

## Cosine Similarity

Cosine similarity measures the angle between two vectors.

It is commonly used to measure semantic similarity.

```text
Similarity → High
     ↓
Vectors point in similar directions
```

## Cosine vs Euclidean Distance

- **Cosine similarity** → compares vector direction
- **Euclidean distance** → measures geometric distance

The appropriate metric depends on the embedding model and retrieval setup.

## Choosing an Embedding Model

Consider:

- Semantic retrieval quality
- Domain
- Language support
- Vector dimensionality
- Latency
- Cost
- Deployment requirements

---

# 4. Vector Databases & Retrieval

## What is a Vector Database?

A vector database stores and searches vector embeddings efficiently.

A stored record may contain:

```text
Vector
Text / Chunk
Metadata
Document ID
```

## Vector Database vs Relational Database

```text
Relational DB
→ rows / columns
→ SQL
→ structured conditions

Vector DB
→ embeddings
→ similarity search
→ semantic retrieval
```

Some relational databases can also support vector search.

## Vector Index

An index is a data structure that makes similarity search faster.

Without an efficient index:

```text
Query
 ↓
Compare with every vector
 ↓
Slow at large scale
```

With an index:

```text
Query
 ↓
Vector Index
 ↓
Relevant Candidates
```

## Approximate Nearest Neighbor (ANN)

ANN methods find approximately similar vectors without comparing the query against every vector.

Benefits:

- Faster retrieval
- Better scalability

Trade-off:

```text
Speed ↔ Search Accuracy
```

## HNSW

**Hierarchical Navigable Small World**

Uses a graph-based structure to efficiently navigate toward similar vectors.

## IVF

**Inverted File Index**

Groups vectors into clusters and searches relevant clusters instead of the entire database.

## Top-k Retrieval

Top-k means retrieving the `k` most relevant results.

Example:

```text
Query
 ↓
Similarity Search
 ↓
Top 5 Results
```

Choosing a larger `k` may improve recall but can introduce irrelevant context.

## Metadata Filtering

Filter documents using metadata such as:

- Date
- Author
- Department
- Category
- Language
- Document type
- Access permissions

## Keyword vs Semantic Search

### Keyword Search

Matches exact words or terms.

Useful for:

- Error codes
- Product IDs
- Technical terms
- Exact phrases

### Semantic Search

Uses embeddings to retrieve based on meaning.

Useful for:

- Natural-language questions
- Conceptual queries
- Similar meanings

## Hybrid Search

Combines keyword and semantic search.

```text
Keyword Search
      +
Semantic Search
      ↓
Result Fusion
      ↓
Better Retrieval
```

---

# 5. Retrieval Quality & Reranking

## What Makes a Document Relevant?

A relevant document should contain information useful for answering the user's query.

Similarity score alone does not guarantee relevance.

## Reranking

Reranking is a second-stage process that reorders initially retrieved documents based on more detailed relevance.

```text
Query
 ↓
Initial Retriever
 ↓
Top 20 Candidates
 ↓
Reranker
 ↓
Top 5 Results
```

## Why Can Similar Chunks Be Poor Context?

A chunk can have high semantic similarity but still:

- Not directly answer the question
- Lack necessary context
- Be too broad
- Contain irrelevant information
- Be redundant

## Query Expansion

Adds related terms or concepts to the query.

```text
Original:
"How can I make RAG faster?"

Expanded:
"RAG latency optimization,
retrieval optimization,
caching, performance"
```

Goal → improve recall.

## Query Rewriting

Transforms the original query into a clearer retrieval-friendly query.

```text
"RAG faster?"

        ↓

"What techniques can reduce
RAG application latency?"
```

## Multi-Query Retrieval

Generate multiple related queries and retrieve results for each.

```text
Original Query
      ↓
 ┌────┼────┐
 Q1   Q2   Q3
 ↓    ↓    ↓
R1   R2   R3
 └────┼────┘
      ↓
 Combine + Deduplicate
      ↓
    Rerank
```

Useful for improving recall and handling different terminology.

---

# 6. Generation & Context Construction

## How Are Retrieved Documents Passed to the LLM?

Retrieved documents are:

```text
Retrieved Chunks
      ↓
Filtering
      ↓
Reranking
      ↓
Deduplication
      ↓
Context Construction
      ↓
Prompt
      ↓
LLM
```

## Context Construction

Context construction means organizing the retrieved information into useful context for the LLM.

Typical steps:

1. Select relevant chunks
2. Remove irrelevant information
3. Remove duplicates
4. Order the information
5. Add useful metadata
6. Fit within the context window

## Preventing Irrelevant Context

Use:

- Better chunking
- Better embeddings
- Metadata filtering
- Hybrid search
- Reranking
- Relevance thresholds
- Deduplication
- Appropriate top-k

## Prompt Template

A prompt template gives the LLM a consistent structure.

```text
Use the following context to answer the question.

Context:
{context}

Question:
{question}

Answer:
```

## RAG and Hallucination

RAG can reduce hallucination by providing external context.

But it does **not eliminate hallucination**.

Possible causes:

- Wrong retrieval
- Missing information
- Irrelevant context
- Conflicting context
- LLM misunderstanding
- Unsupported generation

---

# 7. RAG Evaluation

RAG evaluation should separately evaluate:

```text
RAG Evaluation
      / \
     /   \
Retrieval  Generation
```

## Retrieval Evaluation

Checks whether relevant documents were retrieved.

Common metrics:

- Precision@k
- Recall@k
- Retrieval relevance

## Generation Evaluation

Checks the quality of the generated answer.

Common metrics:

- Faithfulness
- Answer relevance
- Answer quality/correctness

## Precision@k

```text
Precision@k =
Relevant documents in top-k
---------------------------
          k
```

Example:

5 documents retrieved  
3 are relevant

```text
Precision@5 = 3/5 = 60%
```

## Recall@k

```text
Recall@k =
Relevant documents retrieved
----------------------------
Total relevant documents
```

Example:

4 relevant documents exist  
3 were retrieved

```text
Recall@k = 3/4 = 75%
```

### Easy Difference

```text
Precision → How many retrieved results are relevant?

Recall → How many relevant results did we retrieve?
```

## Faithfulness

Checks whether the generated answer is supported by the retrieved context.

```text
Context
   ↓
Answer
```

## Answer Relevance

Checks whether the answer actually addresses the user's question.

```text
Question
   ↓
Answer
```

## Debugging Retrieval vs Generation

```text
Query
 ↓
Retrieved Documents
 ↓
Required information present?
    /          \
  No            Yes
  ↓              ↓
Retrieval     Check Answer
Problem           ↓
              Generation
                Problem
```

## Evaluation Dataset

A useful evaluation dataset can contain:

```text
Question
Relevant Documents
Reference Answer
```

Include:

- Easy queries
- Difficult queries
- Multi-document questions
- Unanswerable questions
- Realistic user queries

---

# 8. Production RAG & Troubleshooting

## Production RAG Architecture

```text
                DATA SOURCES
                     ↓
             Ingestion Pipeline
                     ↓
            Clean + Chunk Documents
                     ↓
                  Embeddings
                     ↓
            Vector DB / Search
                     ↑
                     │
User Query → Query Processing
                     ↓
              Retrieval
                     ↓
               Reranking
                     ↓
          Context Construction
                     ↓
                    LLM
                     ↓
                 Answer
                     ↓
             Monitoring
```

## Production Components

A production system should consider:

- Document ingestion
- Chunking
- Embeddings
- Vector database
- Retrieval
- Reranking
- Context construction
- LLM
- Caching
- Monitoring
- Security
- Error handling

---

# 9. Document Updates & Re-indexing

Use incremental updates when possible.

### New Document

```text
New Document
 ↓
Extract
 ↓
Clean
 ↓
Chunk
 ↓
Embed
 ↓
Insert
```

### Modified Document

```text
Modified Document
 ↓
Reprocess
 ↓
Re-embed
 ↓
Replace Old Vectors
```

### Deleted Document

```text
Deleted Document
 ↓
Remove Associated Chunks/Vectors
```

Useful tracking information:

- Document ID
- Version
- Timestamp
- Content hash

A full re-index may be needed when changing:

- Embedding model
- Chunking strategy
- Major preprocessing
- Index configuration

---

# 10. Reducing RAG Latency

Main techniques:

- Efficient ANN indexes
- Metadata filtering
- Appropriate top-k
- Limit reranking candidates
- Query embedding caching
- Response caching
- Smaller/faster models where appropriate
- Reduce context size
- Remove duplicate chunks
- Parallelize independent operations
- Stream responses

### Latency Optimization

```text
Measure
  ↓
Find Bottleneck
  ↓
Optimize That Stage
  ↓
Measure Again
```

---

# 11. Reducing RAG Cost

Major cost sources:

- LLM tokens
- LLM calls
- Embeddings
- Reranking
- Vector infrastructure

Ways to reduce cost:

- Reduce context size
- Optimize top-k
- Cache repeated queries
- Cache embeddings
- Use appropriate models
- Limit reranking candidates
- Incrementally process changed documents
- Optimize vector search
- Monitor token usage

### Key Principle

```text
Reduce unnecessary computation
            +
Maintain answer quality
            ↓
       Lower Cost
```

---

# 12. Monitoring RAG in Production

Monitor both **system performance** and **RAG quality**.

## System Metrics

- Latency
- Throughput
- Error rate
- Timeout rate
- Token usage
- Cost

## Retrieval Metrics

- Precision@k
- Recall@k
- Retrieval relevance
- Similarity scores

## Generation Metrics

- Faithfulness
- Answer relevance
- Answer quality
- User feedback

## Request Tracing

Track:

```text
Query
 ↓
Embedding
 ↓
Retrieval
 ↓
Reranking
 ↓
Context
 ↓
LLM
 ↓
Answer
```

This makes troubleshooting easier.

---

# 13. Debugging: Correct Document but Wrong Answer

If the correct document is retrieved but the answer is wrong:

```text
Correct Document
       ↓
Does it contain the required information?
       ↓
Check Selected Chunk
       ↓
Check Final Context
       ↓
Check for Conflicting/Irrelevant Context
       ↓
Check Prompt
       ↓
Check Context Size
       ↓
Check LLM Output
```

Important distinction:

> Retrieving the correct document does not necessarily mean the correct context was provided to the LLM.

---

# 14. Debugging: Irrelevant Documents Retrieved

Systematically check:

```text
Query
 ↓
Document Quality
 ↓
Chunking
 ↓
Embedding Model
 ↓
Retrieval Strategy
 ↓
Metadata Filtering
 ↓
Top-k
 ↓
Reranking
 ↓
Deduplication
 ↓
Evaluation
```

Possible improvements:

- Query rewriting
- Query expansion
- Multi-query retrieval
- Better chunking
- Better embeddings
- Hybrid search
- Metadata filtering
- Reranking
- Top-k tuning
- Relevance thresholds
- Deduplication

---

# 15. Complete RAG Pipeline — Interview View

```text
                  DOCUMENTS
                      ↓
             Document Ingestion
                      ↓
               Text Extraction
                      ↓
                  Cleaning
                      ↓
                  Chunking
                      ↓
                 Embeddings
                      ↓
              Vector Database
                      │
                      │
                      │
User Query → Query Processing
                      ↓
                  Retrieval
                      ↓
             Metadata Filtering
                      ↓
                 Reranking
                      ↓
            Context Construction
                      ↓
                    Prompt
                      ↓
                     LLM
                      ↓
                   Answer
                      ↓
          Evaluation + Monitoring
```

---

# 16. Most Important Interview Concepts

Before an interview, make sure you can explain:

### Fundamentals
- What is RAG?
- Why use RAG?
- RAG vs fine-tuning
- RAG vs long context

### Ingestion
- Document ingestion
- Cleaning
- Chunking
- Chunk size
- Chunk overlap
- Chunking strategies

### Embeddings
- Embeddings
- Embedding models
- Dimensionality
- Cosine similarity
- Similarity metrics

### Retrieval
- Vector databases
- Vector indexes
- ANN
- HNSW
- IVF
- Top-k
- Metadata filtering
- Semantic search
- Keyword search
- Hybrid search

### Retrieval Quality
- Relevance
- Reranking
- Query expansion
- Query rewriting
- Multi-query retrieval

### Generation
- Context construction
- Prompt templates
- Context size
- Hallucination
- Grounding

### Evaluation
- Precision@k
- Recall@k
- Faithfulness
- Answer relevance
- Retrieval vs generation evaluation

### Production
- Architecture
- Re-indexing
- Latency
- Cost
- Monitoring
- Troubleshooting

---

# 17. Quick Troubleshooting Cheat Sheet

| Problem | Things to Check |
|---|---|
| Irrelevant documents | Query, chunking, embeddings, retrieval, filters, top-k, reranking |
| Missing relevant document | Recall, chunking, embeddings, query rewriting, hybrid search |
| Correct document but wrong answer | Chunk selection, final context, prompt, conflicting context, LLM |
| Too much context | Top-k, reranking, deduplication, relevance threshold |
| High latency | Retrieval, reranking, LLM, caching, context size |
| High cost | Tokens, model choice, caching, reranking, unnecessary processing |
| Outdated answers | Document updates, indexing, metadata, versioning |
| High hallucination | Retrieval quality, context quality, prompt, faithfulness |
| Production failures | Errors, timeouts, retries, monitoring, infrastructure |

---

# 18. One-Minute RAG Explanation

If an interviewer asks:

**"Explain RAG."**

You can answer:

> RAG stands for Retrieval-Augmented Generation. It combines a retrieval system with an LLM. When a user asks a question, the system converts the query into an embedding and retrieves relevant documents or chunks from a knowledge base, often using a vector database or hybrid search. The retrieved information can then be filtered, reranked, and constructed into context. This context is passed to the LLM along with the user's question, and the LLM generates the final answer. RAG is useful for private, domain-specific, and frequently changing information because the knowledge can be updated in the retrieval system without retraining the LLM.

---

# 19. One-Line Revision

```text
RAG = Retrieve → Filter → Rerank → Construct Context → Generate → Evaluate
```

```text
Good RAG =
Good Data
+ Good Chunking
+ Good Embeddings
+ Good Retrieval
+ Good Reranking
+ Good Context
+ Good Generation
+ Good Evaluation
```

---

# Final Interview Mindset

When debugging a RAG system, always separate the problem into two major stages:

```text
                 RAG
                /   \
               /     \
        Retrieval   Generation
            ↓           ↓
       "Did we get   "Did the LLM
        the right      use it
        information?"  correctly?"
```

This separation makes RAG systems much easier to design, evaluate, optimize, and troubleshoot.
