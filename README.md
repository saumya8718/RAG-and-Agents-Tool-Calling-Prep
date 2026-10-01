# RAG & Agents — Interview Preparation

> A structured interview preparation guide covering **Retrieval-Augmented Generation (RAG)**, **AI Agents**, and **Tool Calling** — from fundamentals to production-level concepts.

---

## 📚 What This Repository Covers

This repository is designed to help you prepare for **AI/ML, Generative AI, and LLM-based interviews**.

The preparation is divided into two major areas:

```text
RAG
│
├── Retrieval-Augmented Generation fundamentals
├── Document ingestion & preprocessing
├── Embeddings
├── Vector databases & retrieval
├── Retrieval quality & reranking
├── Generation & context construction
├── RAG evaluation
└── Production RAG & troubleshooting


Agents / Tool Calling
│
├── Agent fundamentals
├── Tools & tool calling
├── Agent loop & reasoning
├── Memory & state
├── Multi-agent systems
├── Agent frameworks & architecture
├── Agent + RAG + tools
├── Reliability, security & guardrails
└── Production & debugging
```

---

# 🗂️ Repository Structure

```text
rag-agents-interview-prep/
│
├── README.md
│
├── RAG/
│   ├── 00.md
│   ├── 00ToLearn.md
│   ├── 01.md
│   ├── 02.md
│   ├── 03.md
│   ├── 04.md
│   ├── 05.md
│   ├── 06.md
│   ├── 07.md
│   ├── 08.md
│   └── 09_Summary.md
│
└── Agents-Tool-Calling/
    ├── 00.md
    ├── 00ToLearn.md
    ├── 01.md
    ├── 02.md
    ├── 03.md
    ├── 04.md
    ├── 05.md
    ├── 06.md
    ├── 07.md
    ├── 08.md
    └── 09_Summary.md
```

---

# 🔎 RAG

**Retrieval-Augmented Generation (RAG)** combines information retrieval with an LLM.

Instead of relying only on the knowledge already available to the model, a RAG system retrieves relevant information from an external knowledge source and provides it to the LLM as context.

### RAG Learning Path

| File | Topic |
|---|---|
| `00.md` | Introduction & Roadmap |
| `00ToLearn.md` | Prerequisites |
| `01.md` | RAG Fundamentals |
| `02.md` | Document Ingestion & Preprocessing |
| `03.md` | Embeddings |
| `04.md` | Vector Databases & Retrieval |
| `05.md` | Retrieval Quality & Reranking |
| `06.md` | Generation & Context Construction |
| `07.md` | RAG Evaluation |
| `08.md` | Production RAG & Troubleshooting |
| `09_Summary.md` | Quick Interview Revision |

### Core RAG Pipeline

```text
Documents
    ↓
Ingestion
    ↓
Cleaning & Chunking
    ↓
Embeddings
    ↓
Vector Database
    ↓
User Query
    ↓
Query Processing
    ↓
Retrieval
    ↓
Reranking
    ↓
Context Construction
    ↓
LLM
    ↓
Final Answer
```

---

# 🤖 Agents / Tool Calling

An AI agent extends a basic LLM application by allowing the system to dynamically decide what actions to take and which tools to use.

Tool calling allows the LLM to request execution of predefined tools through structured arguments.

### Agents Learning Path

| File | Topic |
|---|---|
| `00.md` | Introduction & Roadmap |
| `00ToLearn.md` | Prerequisites |
| `01.md` | Agent Fundamentals |
| `02.md` | Tools & Tool Calling |
| `03.md` | Agent Loop & Reasoning |
| `04.md` | Memory & State |
| `05.md` | Multi-Agent Systems |
| `06.md` | Agent Frameworks & Architecture |
| `07.md` | Agent + RAG + Tools |
| `08.md` | Reliability, Security & Guardrails |
| `09_Summary.md` | Production, Debugging & Quick Revision |

### Basic Agent Flow

```text
User
 ↓
Agent / LLM
 ↓
Decide
 ↓
 ┌───────────────┐
 │               │
 ↓               ↓
Answer        Call Tool
                  ↓
             Tool Result
                  ↓
             Update State
                  ↓
             Decide Again
                  ↓
             Final Answer
```

---

# 🔗 RAG + Agents

The two sections are designed to build on each other.

```text
RAG
 ↓
Learn Retrieval
 ↓
Learn Embeddings
 ↓
Learn Vector Databases
 ↓
Learn Reranking
 ↓
Learn RAG Evaluation
 ↓
Learn Production RAG
 ↓
        + 
        ↓
Agents
 ↓
Learn Tool Calling
 ↓
Learn Agent Loop
 ↓
Learn State & Memory
 ↓
Learn Multiple Tools
 ↓
Use RAG as a Tool
 ↓
Build Agentic Systems
 ↓
Production Agents
```

### Simple Mental Model

```text
RAG
→ Retrieve information

Tool Calling
→ Use external capabilities

Agent
→ Decide what to do

Agent + RAG
→ Decide when and how to retrieve information
```

---

# 🎯 What You Will Be Able to Explain

After completing this repository, you should be able to explain:

### RAG

- What RAG is
- Why RAG is used
- Document ingestion
- Chunking strategies
- Embeddings
- Vector similarity
- Vector databases
- Metadata filtering
- Hybrid search
- Reranking
- Context construction
- RAG evaluation
- RAG failure modes
- Production RAG architecture
- RAG latency and cost optimization

### Agents

- What an AI agent is
- LLM vs workflow vs agent
- Agent components
- Tool calling
- Function calling
- Tool schemas
- Structured tool arguments
- Agent loops
- Stopping conditions
- Memory and state
- Short-term vs long-term memory
- Multi-agent systems
- Manager-worker architecture
- LangChain
- LangGraph
- Agent + RAG
- Multiple tools
- Research agents
- Tool validation
- Authorization
- Prompt injection
- Guardrails
- Agent reliability
- Agent monitoring
- Agent evaluation
- Production architecture

---

# 🧭 Recommended Learning Order

Follow the repository in this order:

```text
                    START
                      ↓
             RAG/00ToLearn.md
                      ↓
                 RAG/01.md
                      ↓
              RAG/02 → RAG/03
                      ↓
              RAG/04 → RAG/05
                      ↓
              RAG/06 → RAG/07
                      ↓
                 RAG/08.md
                      ↓
              RAG/09_Summary.md
                      ↓
            Agents/00ToLearn.md
                      ↓
            Agents/01 → Agents/02
                      ↓
            Agents/03 → Agents/04
                      ↓
            Agents/05 → Agents/06
                      ↓
            Agents/07 → Agents/08
                      ↓
              Agents/09_Summary.md
                      ↓
                    DONE
```

---

# 💡 How to Use This Repository

For each topic:

1. Read the concept.
2. Understand the architecture/flow.
3. Study the examples.
4. Try explaining the concept without looking at the answer.
5. Practice the **Interview Answer** section.
6. Revise the summary files before interviews.

A useful approach is:

```text
Learn
 ↓
Understand
 ↓
Explain in your own words
 ↓
Practice questions
 ↓
Build a small implementation
 ↓
Revise
```

---

# 🛠️ Practical Focus

This repository focuses on understanding **how these systems actually work**, rather than only memorizing definitions.

Important implementation concepts include:

```text
RAG
├── Ingestion
├── Chunking
├── Embeddings
├── Retrieval
├── Reranking
└── Generation

Agents
├── Tool Definitions
├── Tool Calling
├── Agent Loop
├── State
├── Tool Execution
├── Validation
├── Guardrails
└── Monitoring
```

---

# 📌 Interview Preparation Strategy

For every important concept, try to answer three levels:

### Level 1 — Definition

> What is it?

### Level 2 — Working

> How does it work?

### Level 3 — Production

> What can go wrong and how would you handle it?

For example:

```text
What is RAG?
      ↓
How does RAG work?
      ↓
How would you build production RAG?
      ↓
What if retrieval returns irrelevant documents?
      ↓
How would you debug and evaluate it?
```

The same approach applies to agents:

```text
What is an AI agent?
      ↓
How does tool calling work?
      ↓
How does the agent loop work?
      ↓
What if the agent chooses the wrong tool?
      ↓
How do you make it safe and production-ready?
```

---

# 🚀 Final Goal

The goal of this repository is not just to memorize interview answers.

It is to develop a clear mental model of:

```text
                    LLM APPLICATIONS
                          │
             ┌────────────┴────────────┐
             ↓                         ↓
            RAG                      AGENTS
             │                         │
      Retrieve Knowledge          Take Actions
             │                         │
             └────────────┬────────────┘
                          ↓
                  Agentic RAG Systems
                          ↓
                  Production Systems
```

By the end, you should be comfortable explaining **what these systems are, how they work, how to implement their core components, and how to reason about reliability, security, evaluation, cost, and performance.**

---

## ⭐ Quick Revision

```text
RAG
= Retrieve relevant information → Give it to LLM → Generate answer

TOOL CALLING
= LLM selects a predefined tool → Application executes it → Result returns to LLM

AGENT
= LLM + Tools + State + Instructions + Agent Loop

AGENT + RAG
= Agent dynamically decides when to retrieve information

PRODUCTION
= Correctness + Safety + Reliability + Observability
  + Cost + Latency
```

---

## 📖 Start Here

### RAG
➡️ `RAG/00.md`

### Agents / Tool Calling
➡️ `Agents-Tool-Calling/00.md`

---

## 📌 Repository Status

```text
RAG                    ✅
Agents / Tool Calling  ✅
Interview Questions    ✅
Quick Revision         ✅
Production Concepts    ✅
```

---

**Built for structured preparation in RAG, AI Agents, Tool Calling, and modern LLM application development.**
