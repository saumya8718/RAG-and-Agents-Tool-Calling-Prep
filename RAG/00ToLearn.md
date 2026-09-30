# What to Learn Before RAG

Before starting the RAG interview questions, make sure you understand
the following concepts.

## 1. LLM Basics

Understand:

- What is an LLM?
- How an LLM generates a response
- Tokens
- Context window
- Prompts
- System, user, and assistant messages
- Hallucination

---

## 2. Basic NLP Concepts

Understand:

- Text preprocessing
- Tokens
- Documents
- Sentences and paragraphs
- Semantic similarity
- Semantic search

---

## 3. Embeddings

You don't need to know the mathematics in depth initially.

Understand:

- What an embedding is
- Why text is converted into vectors
- Vector representation
- Similarity between vectors
- Cosine similarity

---

## 4. Basic Vector / Search Concepts

Understand:

- Vectors
- Dimensions
- Similarity search
- Nearest-neighbor search
- Top-k results
- Indexing

These concepts will make vector databases and retrieval much easier
to understand.

---

## 5. Basic Database Concepts

Understand:

- What is a database?
- Tables and records
- Indexes
- Queries
- Metadata
- Filtering

You don't need advanced DBMS knowledge specifically for RAG.

---

## 6. APIs and Python

You should be comfortable with:

- Python functions
- Lists and dictionaries
- JSON
- APIs
- HTTP requests
- Reading API responses

---

## 7. Basic LLM Application Flow

Understand this first:

User
 ↓
Application
 ↓
LLM API
 ↓
LLM Response

RAG extends this architecture by adding a retrieval step:

User
 ↓
Application
 ↓
Retriever
 ↓
Relevant Context
 ↓
LLM
 ↓
Response

---

## Recommended Learning Order

LLM Basics
     ↓
NLP Basics
     ↓
Embeddings
     ↓
Vector Similarity
     ↓
Vector Databases
     ↓
Retrieval
     ↓
RAG
     ↓
Advanced RAG
     ↓
RAG Evaluation
     ↓
Production RAG
