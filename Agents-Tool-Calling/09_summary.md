# Agents / Tool Calling — Quick Revision Summary

# 1. What is an AI Agent?

An **AI agent** is a system that uses an LLM to:

1. Understand a goal
2. Decide what action to take
3. Use tools when required
4. Observe the result
5. Update its state
6. Continue until the task is completed or a stopping condition is reached

### Basic Agent Loop

~~~text
User Request
     ↓
Understand Goal
     ↓
Decide Next Action
     ↓
 ┌───┴─────────────┐
 ↓                 ↓
Answer          Call Tool
                    ↓
               Tool Result
                    ↓
              Update State
                    ↓
              Decide Again
                    ↓
              Final Answer
~~~

### Remember

```text
Agent = LLM + Instructions + Tools + State/Memory + Agent Loop
```

---

# 2. LLM vs Workflow vs Agent

| Concept | Main Idea |
|---|---|
| LLM | Generates output from input |
| Workflow | Follows a predefined sequence of steps |
| Agent | Dynamically decides what to do next |

### Example

```text
Workflow:
Input → Search → Summarize → Answer

Agent:
Input → Decide
          ↓
       Search?
          ↓
       Database?
          ↓
       Calculator?
          ↓
       Answer?
```

---

# 3. When Should You Use an Agent?

Use an agent when the task requires:

- Multiple steps
- Dynamic decision-making
- Multiple tools
- Different possible execution paths
- Interaction with external systems
- Adaptation based on tool results

For simple and predictable tasks, a normal function or deterministic workflow is often easier to control.

### Interview Line

> Use an agent when the next action cannot be completely predetermined and must be selected dynamically based on the current task state.

---

# 4. What is a Tool?

A **tool** is an external capability that an agent can use.

Examples:

```text
Calculator
Search API
Database
Weather API
Payment API
RAG Retriever
Email Service
Code Execution
```

The important distinction is:

```text
LLM → DECIDES to use tool
Application → EXECUTES the tool
```

The LLM itself does not automatically execute arbitrary application functions.

---

# 5. Tool Calling / Function Calling

Tool calling allows an LLM to request that the application execute a predefined tool using structured arguments.

### Complete Flow

~~~text
User
 ↓
Application
 ↓
LLM + Tool Definitions
 ↓
LLM selects tool
 ↓
Structured Arguments
 ↓
Application validates arguments
 ↓
Application executes tool
 ↓
Tool Result
 ↓
LLM
 ↓
Final Response
~~~

### Important

```text
LLM decision ≠ Tool execution
```

The application remains responsible for execution, validation, authorization and safety.

---

# 6. Tool Schema

A tool definition should generally describe:

- Tool name
- Tool description
- Parameters
- Parameter types
- Required fields
- Constraints
- Allowed values/enums where applicable

Example:

~~~json
{
  "name": "get_weather",
  "description": "Get weather for a city",
  "parameters": {
    "city": {
      "type": "string"
    }
  }
}
~~~

Good tool descriptions help the model select the correct tool.

---

# 7. Structured Tool Input / Output

Tool arguments and results should preferably use structured formats such as JSON.

### Example

~~~json
{
  "city": "Delhi",
  "unit": "celsius"
}
~~~

Structured data makes validation, parsing and debugging easier.

---

# 8. Agent Loop

The **agent loop** is the repeated process through which an agent performs a task.

~~~text
Understand
    ↓
Decide
    ↓
Act
    ↓
Observe
    ↓
Update State
    ↓
Decide Again
    ↓
Stop
~~~

### The loop may stop when:

- Task is completed
- Final answer is available
- Maximum steps reached
- Timeout occurs
- Tool failure cannot be recovered
- Token/cost limit is reached
- Other safety limits are triggered

---

# 9. Multi-Step Reasoning

An agent may need multiple actions to complete one task.

Example:

```text
User:
"Find the price of a product and calculate the total for 5 units."

Step 1 → Search product price
Step 2 → Get price
Step 3 → Call calculator
Step 4 → Generate answer
```

The important interview point is that the agent performs **multiple observable actions and decisions** rather than simply generating one response.

---

# 10. Infinite Agent Loops

An agent can repeatedly call the same tool because of:

- Missing stopping condition
- Poor tool result
- Incorrect state update
- Repeated tool selection
- Failed retry handling
- Confusing instructions

### Prevention

Use:

```text
Maximum steps
Maximum retries
Maximum repeated tool calls
Timeout
Token budget
Cost budget
Tool-call limits
```

---

# 11. Memory vs State vs Conversation History

These concepts should not be confused.

### Conversation History

The chronological messages exchanged between user and assistant.

### State

The current information needed to continue the task.

Example:

```text
Current step
Selected tool
Tool results
Task progress
Intermediate results
Next action
```

### Memory

Useful information retained and retrieved for future use.

### Important Distinction

```text
Memory ≠ Entire Conversation History
```

An agent should not blindly pass the complete conversation every time.

---

# 12. Short-Term vs Long-Term Memory

### Short-Term Memory

Used for the current task or session.

Examples:

- Current conversation
- Current task state
- Intermediate tool results

### Long-Term Memory

Information retained across sessions.

Examples:

- Persistent user preferences
- Previously stored information
- Historical task information

Both should be selectively retrieved rather than blindly included.

---

# 13. Problems With Too Much Context

Passing too much conversation/history can cause:

- Context-window problems
- Higher token cost
- Higher latency
- Irrelevant information
- Conflicting information
- Reduced attention to important details
- Repeated tool outputs

### Better Approach

```text
Summarization
+
Relevant Context Selection
+
Structured State
+
Selective Retrieval
```

---

# 14. Multi-Agent Systems

A **multi-agent system** contains multiple agents working together.

Each agent can have:

- Different instructions
- Different tools
- Different responsibilities
- Different expertise

### Example

~~~text
                  Manager Agent
                       ↓
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   Research Agent   Data Agent    Writing Agent
        ↓              ↓              ↓
        └──────────────┼──────────────┘
                       ↓
                 Final Response
~~~

### Advantages

- Specialization
- Parallel work
- Modular architecture
- Different tool access

### Problems

- Higher complexity
- Higher latency
- Higher cost
- Coordination problems
- Communication overhead
- More difficult debugging

---

# 15. Manager-Worker Architecture

A common multi-agent design is:

```text
User
 ↓
Manager Agent
 ↓
Task Decomposition
 ↓
Specialized Agents
 ├── Researcher
 ├── Data Analyst
 └── Writer
 ↓
Shared State / Results
 ↓
Manager
 ↓
Final Answer
```

The manager delegates tasks and combines the results.

---

# 16. LangChain

**LangChain** is a framework for building LLM applications.

It provides components for things such as:

- LLMs
- Prompts
- Tools
- Agents
- Retrieval
- Output parsing
- External integrations

### Important

```text
LangChain ≠ LLM
```

It is an application/orchestration framework around LLM-based systems.

---

# 17. LangGraph

**LangGraph** is useful for building stateful, multi-step and agentic workflows using graph-based execution.

Basic concepts:

```text
Nodes → Actions
Edges → Transitions
State → Shared information
```

Example:

~~~text
START
  ↓
Agent
  ↓
Tool
  ↓
Update State
  ↓
Agent
  ↓
END
~~~

It is useful when an application requires branching, loops and explicit state management.

---

# 18. Chain vs Workflow vs Agent

| Type | Execution |
|---|---|
| Chain | Fixed sequence |
| Workflow | Developer-defined process with conditions/branches |
| Agent | Dynamically chooses next action |

### Easy Memory Trick

```text
Chain     → Fixed steps
Workflow  → Controlled steps
Agent     → Dynamic steps
```

---

# 19. When Should You Avoid an Agent Framework?

A framework may be unnecessary when the task is:

- Simple
- Small
- Deterministic
- Highly controlled
- Latency-sensitive

For a simple API call, direct application code may be easier than introducing a complete agent framework.

---

# 20. Simple Tool-Calling Agent Without a Framework

The basic architecture is:

~~~text
User
 ↓
Application
 ↓
LLM + Tool Schemas
 ↓
Tool Call?
 ├── No → Final Answer
 │
 └── Yes
       ↓
   Validate Tool
       ↓
   Execute Tool
       ↓
   Tool Result
       ↓
   LLM
       ↓
   Repeat / Final Answer
~~~

### Conceptual Formula

```text
LLM
+
Tool Schemas
+
Application Code
+
Agent Loop
+
State
+
Validation/Safety
```

---

# 21. Agent + RAG

An agent can use RAG as one of its tools.

Example:

```text
Tool:
search_knowledge_base(query)
```

Internally this tool may perform:

```text
Query
 ↓
Embedding
 ↓
Vector / Hybrid Search
 ↓
Metadata Filtering
 ↓
Reranking
 ↓
Relevant Chunks
```

The agent decides **when** to use this retrieval capability.

---

# 22. RAG Pipeline vs Agent With RAG

### Traditional RAG

```text
User Query
 ↓
Retrieve
 ↓
Rerank
 ↓
Context
 ↓
LLM
 ↓
Answer
```

### Agent + RAG

```text
User
 ↓
Agent
 ↓
Decide:
 ├── Answer directly
 ├── Use RAG
 ├── Search Web
 ├── Query Database
 └── Use Calculator
        ↓
    Observe Result
        ↓
    Decide Again
        ↓
    Final Answer
```

### Key Difference

```text
RAG = Retrieval capability

Agent = Decision + Orchestration
```

---

# 23. How Does an Agent Decide Whether to Retrieve?

The decision can depend on:

- User query
- Available tools
- Tool descriptions
- Instructions
- Current state
- Existing context
- Whether information is already available
- Whether information is private/domain-specific
- Whether external/current information is required

Example:

```text
"What is 2 + 2?"
→ Calculator may not be necessary.

"What is our company's leave policy?"
→ Internal RAG may be required.

"What is the current price?"
→ Current external source/API may be required.
```

---

# 24. Multiple Tools

An agent may combine tools such as:

```text
Search
Database
RAG
Calculator
External API
```

### Sequential Execution

Used when one result is needed for the next step.

```text
Search
 ↓
Get Result
 ↓
Calculator
 ↓
Answer
```

### Parallel Execution

Independent tools may sometimes run in parallel.

```text
        ┌── Search
Agent ──┼── Database
        └── API
             ↓
       Combine Results
```

---

# 25. Research Agent

A research agent can follow:

```text
Understand Question
       ↓
Create Research Plan
       ↓
Use Multiple Sources
       ↓
Collect Evidence
       ↓
Validate / Organize
       ↓
Need More Research?
   ┌──────┴──────┐
  Yes            No
   ↓              ↓
Research More   Synthesize
                  ↓
             Final Answer
```

Important production considerations:

- Track sources
- Avoid duplicate information
- Validate results
- Limit tool calls
- Handle tool failures
- Keep intermediate state
- Ground final answer in collected evidence

---

# 26. Major Agent Failure Modes

Common failures include:

```text
Wrong Tool
Invalid Arguments
Hallucinated Information
Infinite Loops
Poor Planning
Tool Failure
Timeout
Unauthorized Action
Prompt Injection
Untrusted Tool Output
High Cost
High Latency
Data Leakage
```

### Key Principle

> Never assume that because the LLM selected a tool, the requested action is safe.

---

# 27. Tool Argument Validation

Before executing a tool:

```text
Tool Call
   ↓
Parse
   ↓
Schema Validation
   ↓
Type Validation
   ↓
Required Fields
   ↓
Range / Enum Checks
   ↓
Business Rules
   ↓
Authorization
   ↓
Execute
```

### Important

```text
Valid JSON ≠ Valid Business Action
```

---

# 28. Authorization

An LLM should never be the final authority for permissions.

Use application-side:

- Authentication
- Authorization
- Least privilege
- Tool allowlists
- User/resource-level checks
- Rate limits
- Confirmation for sensitive actions

### Remember

```text
LLM decision ≠ Authorization
Tool selection ≠ Permission
```

---

# 29. Prompt Injection

**Prompt injection** occurs when untrusted content attempts to influence or override the instructions governing the agent.

This is especially important when the agent can call tools.

### Potential Sources

```text
Web Pages
Documents
User Input
Database Content
Retrieved RAG Documents
External APIs
```

### Defense

- Treat external content as untrusted data
- Keep system instructions separate
- Validate tool calls
- Use least-privilege permissions
- Use allowlists
- Require confirmation for sensitive actions
- Monitor suspicious behavior

---

# 30. Why Are Tool Outputs Untrusted?

Tool outputs can contain:

- Incorrect information
- Malicious content
- Unexpected data
- Prompt injection
- Invalid formats
- Stale information

Therefore:

```text
Tool Output = Data
NOT
Trusted Instruction
```

---

# 31. Safe Database/API Access

Do not give an agent unrestricted access.

Prefer:

```text
Agent
 ↓
Narrow Tool
 ↓
Validation
 ↓
Authorization
 ↓
Database/API
```

Use:

- Narrowly scoped tools
- Read-only access where possible
- Input validation
- Authorization
- Rate limits
- Timeouts
- Secret management
- Audit logs

Avoid exposing unrestricted credentials or unrestricted database operations directly to the model.

---

# 32. Tool Failures

Possible failures:

```text
Timeout
API Error
Network Error
Malformed Response
Rate Limit
Authentication Failure
Temporary Service Failure
```

Handle them using:

- Timeouts
- Bounded retries
- Exponential backoff where appropriate
- Response validation
- Fallbacks where appropriate
- Graceful failure
- Logging

Never let the agent endlessly retry.

---

# 33. Debugging Wrong Tool Selection

If an agent selects the wrong tool:

### Check

1. User request
2. Ambiguity in request
3. Tool descriptions
4. Tool names
5. Tool schemas
6. System instructions
7. Available tools
8. Complete execution trace
9. Tool-selection test cases

### Trace

```text
User Request
 ↓
Instructions
 ↓
Available Tools
 ↓
LLM Decision
 ↓
Selected Tool
 ↓
Arguments
 ↓
Tool Result
```

If two tools have very similar descriptions, make their responsibilities more distinct.

---

# 34. Debugging Repeated Tool Calls

If the agent repeatedly calls the same tool:

Check:

- Agent trace
- Tool arguments
- Tool results
- State updates
- Stopping conditions
- Retry logic
- Whether the tool result actually tells the agent the task is complete

Add:

```text
Maximum Steps
Maximum Retries
Maximum Repeated Tool Calls
Timeout
```

---

# 35. Reducing Agent Latency

Latency can come from:

```text
LLM Calls
Tool Calls
Database/Search
Network
Large Context
Reranking
Retries
Sequential Execution
```

### Optimization

- Reduce unnecessary LLM calls
- Reduce unnecessary tool calls
- Parallelize independent tools
- Use appropriate/faster models
- Optimize search/database operations
- Cache repeated results
- Reduce context size
- Stream responses
- Limit retries

### First Rule

> Measure the bottleneck before optimizing it.

---

# 36. Controlling Agent Cost

Agent cost can increase because of:

```text
Multiple LLM Calls
Large Prompts
Large Tool Results
Many Agent Steps
Repeated Calls
Expensive Models
Retries
```

### Controls

```text
Maximum Steps
Maximum Tool Calls
Token Budget
Cost Budget
Timeout
Retry Limit
Context Limits
Caching
Model Selection
```

### Monitor

- Cost/request
- Tokens/request
- LLM calls/request
- Tool calls/request

---

# 37. Monitoring an Agent in Production

### System Metrics

- Latency
- Throughput
- Error rate
- Timeout rate
- Availability
- Token usage
- Cost

### Agent Metrics

- Steps/request
- Tool calls/request
- Tool-selection errors
- Repeated tool calls
- Task completion rate
- Failure rate

### Tool Metrics

- Calls
- Success/failure
- Latency
- Timeouts
- Retries

### LLM Metrics

- Input tokens
- Output tokens
- Model
- Latency
- Number of calls
- Cost

### Quality Metrics

- Task success
- Tool-selection accuracy
- Answer correctness
- Groundedness
- User feedback

---

# 38. Distributed Tracing

For production debugging, trace the complete request.

```text
Request ID / Trace ID
        ↓
User Request
        ↓
LLM Call
        ↓
Tool Selection
        ↓
Tool Call
        ↓
Tool Result
        ↓
LLM Call
        ↓
Final Answer
```

This makes it easier to identify where failures occur.

Sensitive information should be protected in logs.

---

# 39. Evaluating an Agent

An agent should not be evaluated only on whether it generates a good-looking answer.

Evaluate:

### Task Success

Did it actually complete the task?

### Tool Selection

Did it choose the correct tool?

### Tool Arguments

Were the arguments correct?

### Final Answer

Was the final answer correct?

### Groundedness

For RAG-based systems, was the answer supported by retrieved information?

### Efficiency

- Number of steps
- Tool calls
- Latency
- Cost

### Robustness

Test:

- Missing information
- Invalid input
- Tool failure
- Timeout
- Ambiguous requests
- Malformed responses
- Prompt injection

---

# 40. Production-Grade Agent Architecture

A production architecture can be represented as:

~~~text
                User / Application
                       ↓
              Authentication
                       ↓
                Rate Limiting
                       ↓
              Agent Orchestrator
                       ↓
              ┌───────────────┐
              │   LLM + State │
              └───────┬───────┘
                      ↓
             Tool Validation
                      ↓
             Authorization
                      ↓
                 Tool Gateway
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      RAG          Database       APIs
        ↓             ↓             ↓
        └─────────────┼─────────────┘
                      ↓
                 Tool Results
                      ↓
                   Agent
                      ↓
               Final Response
                      ↓
              Observability
```

---

# 41. Important Production Layers

A production-grade agent should consider:

```text
Authentication
Authorization
Agent Orchestration
State Management
LLM Layer
Tool Gateway
External Tools
Reliability
Security
Observability
Evaluation
```

### Tool Gateway

The tool gateway can enforce:

- Schema validation
- Authorization
- Rate limits
- Execution constraints
- Logging

---

# 42. Production State

State may contain:

```text
User Request
Current Step
Selected Tool
Tool Arguments
Tool Results
Intermediate Results
Task Progress
Final Result
```

Do not blindly pass the entire conversation to every LLM call.

Use relevant state and context.

---

# 43. Agent Safety Mental Model

Remember these four rules:

```text
LLM decision       ≠ Authorization

Tool selection     ≠ Permission

Tool output        ≠ Trusted Instruction

Valid schema       ≠ Valid Business Action
```

These are extremely important in agent interviews.

---

# 44. RAG vs Agent vs Agent + RAG

| Concept | Main Purpose |
|---|---|
| RAG | Retrieve relevant information |
| Agent | Decide and execute actions dynamically |
| Agent + RAG | Dynamically decide when/how to retrieve information |

### Simple Memory Trick

```text
RAG      → Retrieve
Agent    → Decide + Act
Agent+RAG → Decide + Retrieve + Act
```

---

# 45. Most Important Interview Questions

Before an interview, make sure you can answer these without notes:

### Fundamentals

```text
What is an AI agent?
LLM vs workflow vs agent?
Main components of an agent?
When should you use an agent?
```

### Tool Calling

```text
What is a tool?
What is tool calling?
Why can't an LLM directly execute functions?
How does tool selection happen?
What does a tool schema contain?
What happens with invalid arguments?
```

### Agent Loop

```text
What is the agent loop?
How does an agent stop?
How are multiple tools handled?
Why do infinite loops happen?
How do you limit agent steps?
```

### Memory & State

```text
What is agent memory?
Why does an agent need state?
Memory vs conversation history?
Short-term vs long-term memory?
How is state maintained across tool calls?
```

### Multi-Agent

```text
What is a multi-agent system?
When would you use multiple agents?
How do agents communicate?
What are the advantages/problems?
How does manager-worker architecture work?
```

### Frameworks

```text
What is LangChain?
What is LangGraph?
Chain vs workflow vs agent?
What is a state graph?
When should you avoid a framework?
How would you build an agent without a framework?
```

### Agent + RAG

```text
How can an agent use RAG?
RAG pipeline vs agent with retrieval?
When should an agent retrieve?
How can an agent combine multiple tools?
How would you design a research agent?
```

### Security

```text
What can go wrong with tool calling?
How do you validate tool arguments?
How do you prevent unauthorized actions?
What is prompt injection?
Why are tool outputs untrusted?
How do you safely access a database/API?
How do you handle tool failures?
```

### Production

```text
How do you debug wrong tool selection?
How do you debug repeated tool calls?
How do you reduce latency?
How do you control cost?
How do you monitor an agent?
How do you evaluate an agent?
How do you design a production-grade agent?
```

---

# 46. One-Minute Agent Explanation

If an interviewer asks:

> **"Explain AI agents."**

You can answer:

> An AI agent is an LLM-based system that can dynamically decide what actions to take to complete a task. Unlike a simple LLM application or fixed workflow, an agent can select tools such as APIs, databases, search systems, calculators or RAG based on the current task. The basic loop is understand the goal, decide an action, execute a tool, observe the result, update state and decide again until the task is completed or a stopping condition is reached. In production, the application must also handle validation, authorization, security, retries, timeouts, monitoring, cost and latency.

---

# 47. One-Minute Tool Calling Explanation

> Tool calling allows an LLM to request the execution of a predefined function using structured arguments. The LLM does not directly execute the function. Instead, the application receives the tool call, validates the arguments, checks authorization and executes the tool. The result is then returned to the LLM, which can use it to produce the final response or decide on another action.

### Flow

```text
User
 ↓
LLM
 ↓
Tool Selection
 ↓
Structured Arguments
 ↓
Application Validation
 ↓
Tool Execution
 ↓
Tool Result
 ↓
LLM
 ↓
Final Answer
```

---

# 48. One-Minute Production Agent Explanation

> A production-grade agent should not only be capable of completing a task; it should be safe, reliable, observable and cost-efficient. I would separate the agent orchestrator, state management, LLM layer, tool gateway and external services. The tool gateway would perform schema validation, authorization and execution controls. I would add timeouts, bounded retries, rate limits and maximum agent steps. For observability, I would track traces, LLM calls, tool calls, latency, tokens, cost and failures. Finally, I would evaluate task success, tool selection, argument correctness, answer quality and robustness against failures and security threats.

---

# 49. Final Cheat Sheet

```text
AI AGENT
= LLM + Instructions + Tools + State + Loop

TOOL
= External capability used by the agent

TOOL CALLING
= LLM requests a structured tool execution

APPLICATION
= Validates + Authorizes + Executes tools

AGENT LOOP
= Decide → Act → Observe → Update → Repeat

STATE
= Current task information and progress

MEMORY
= Useful information retained/retrieved

WORKFLOW
= Developer-defined execution

AGENT
= Dynamically selected execution

RAG
= Retrieval capability

MULTI-AGENT
= Multiple specialized agents working together

LANGCHAIN
= LLM application framework

LANGGRAPH
= Stateful graph-based agent/workflow framework

PROMPT INJECTION
= Untrusted content attempting to influence agent behavior

PRODUCTION
= Safety + Reliability + Observability + Evaluation
  + Cost + Latency
```

---

# 50. Final Interview Checklist

Before considering Agents / Tool Calling complete, make sure you can explain:

- [ ] What an AI agent is
- [ ] LLM vs workflow vs agent
- [ ] Components of an agent
- [ ] Tool calling
- [ ] Tool schemas
- [ ] Structured arguments
- [ ] Complete tool-calling flow
- [ ] Agent loop
- [ ] Stopping conditions
- [ ] Infinite loops
- [ ] Memory vs state vs history
- [ ] Short-term vs long-term memory
- [ ] Multi-agent architecture
- [ ] Manager-worker pattern
- [ ] LangChain
- [ ] LangGraph
- [ ] Chain vs workflow vs agent
- [ ] Agent + RAG
- [ ] Multiple tools
- [ ] Research agent
- [ ] Tool validation
- [ ] Authorization
- [ ] Prompt injection
- [ ] Untrusted tool outputs
- [ ] Tool failures
- [ ] Debugging wrong tool selection
- [ ] Debugging repeated calls
- [ ] Latency optimization
- [ ] Cost control
- [ ] Production monitoring
- [ ] Agent evaluation
- [ ] Production-grade architecture

---

# Final Mental Model

```text
                    ┌───────────────┐
                    │     USER      │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │     AGENT     │
                    │               │
                    │  LLM + State  │
                    │  + Agent Loop │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │ DECIDE ACTION │
                    └───────┬───────┘
                            ↓
        ┌───────────────────┼───────────────────┐
        ↓                   ↓                   ↓
      RAG                Database             API
        ↓                   ↓                   ↓
        └───────────────────┼───────────────────┘
                            ↓
                     Tool Results
                            ↓
                    Update State
                            ↓
                     Decide Again
                            ↓
                      Final Answer
```

### The Core Idea

> **RAG gives an AI system the ability to retrieve information. Tool Calling gives it the ability to interact with external capabilities. An Agent adds the ability to dynamically decide which actions to take and in what sequence.**

### The Production Idea

> **A good agent is not just one that can perform a task. It must perform the task correctly, safely, reliably, observably, and within acceptable cost and latency limits.**
