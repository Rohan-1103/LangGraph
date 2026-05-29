# LangGraph Fundamentals — Interview Notes + Important Questions

# Components of LangGraph

## 1. Nodes

### Definition

Nodes are executable functions in a graph that process state and return updated state.

### Example

```python id="l6oh4y"
def chatbot(state):
    return {
        "messages": [
            llm.invoke(state["messages"])
        ]
    }
```

### Interview One-Liner

> Nodes are the computation units of LangGraph.

---

## 2. Edges

### Definition

Edges define the flow between nodes.

They determine:

* Which node executes next
* Conditional routing logic

### Types

* Normal edges
* Conditional edges

### Example

```python id="xjlwm5"
builder.add_edge("chatbot", "tools")
```

### Interview One-Liner

> Edges control execution flow between nodes.

---

## 3. State

### Definition

State is the shared memory/data passed between nodes.

Usually implemented using:

* TypedDict
* Pydantic

### Example

```python id="r4yr1d"
class State(TypedDict):
    messages: list
```

### Interview One-Liner

> State acts as the shared memory layer of the graph.

---

# Why TypedDict and Annotated?

## Why TypedDict?

### Answer

TypedDict provides:

* Structured state schema
* Type safety
* Lightweight implementation

---

## Why Annotated?

### Answer

Annotated allows attaching reducers or metadata to state keys.

---

## Important Example

```python id="gz5shw"
from typing_extensions import TypedDict, Annotated
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[
        list,
        add_messages
    ]
```

---

# What is `add_messages` Reducer?

### Answer

`add_messages` appends new messages instead of overwriting existing ones.

Without reducer:

```python id="jv8f5u"
messages = new_messages
```

With reducer:

```python id="c3qgw8"
messages += new_messages
```

---

# Reducers (`add_messages`)

## What are Reducers?

### Answer

Reducers define how state updates are merged.

---

## Why needed?

### Answer

Because multiple nodes may update the same state key.

Reducers avoid overwriting previous values.

---

## Most Important Reducer

```python id="0u31gn"
add_messages
```

Used for chat history accumulation.

---

# Node Implementation

## Basic Node Structure

```python id="ytn5nd"
def chatbot(state: State):
    response = llm.invoke(
        state["messages"]
    )

    return {
        "messages": [response]
    }
```

---

# Graph

## What is a Graph?

### Answer

A graph is the workflow structure connecting nodes and edges.

It defines:

* Execution flow
* State transitions
* Routing logic

---

# StateGraph

## What is StateGraph?

### Answer

StateGraph is LangGraph’s primary graph class for building stateful workflows.

---

## Syntax

```python id="7s1uqm"
from langgraph.graph import StateGraph

builder = StateGraph(State)
```

---

## Why important?

### Answer

StateGraph:

* Maintains shared state
* Enables multi-step reasoning
* Supports conditional routing

---

# Graph API

## What is Graph API?

### Answer

Graph API is the node-edge based workflow building approach in LangGraph.

---

## Features

* Explicit nodes
* Explicit edges
* Stateful execution
* Conditional routing

---

# Extracting Only AI Messages

## Important Code

```python id="7w3e4e"
for event in graph.stream(
    {"messages":"Hi"}
):
    for value in event.values():
        print(
            value["messages"][-1].content
        )
```

---

## Why does this work?

### Answer

Because:

```python id="pmt36x"
value["messages"][-1]
```

extracts the latest message added to state, which is usually the latest AIMessage.

---

## Why use `stream()`?

### Answer

Streaming:

* Provides real-time outputs
* Improves responsiveness
* Enables incremental updates

---

# Conditional Edges

## What are Conditional Edges?

### Answer

Conditional edges dynamically route execution based on runtime conditions.

---

## Example

```python id="o8v06w"
builder.add_conditional_edges(
    "chatbot",
    tools_condition
)
```

---

## Why important?

### Answer

They enable:

* Decision making
* Tool routing
* Dynamic workflows

---

# Why StateGraph Sometimes Fails at Multiple Tool Calls?

## Important Interview Question

### Answer

Basic StateGraph workflows are often linear and may not properly coordinate multiple sequential or parallel tool calls.

Example:

```text id="q6c4lk"
Get AI news AND multiply 5 by 10
```

The graph may:

* Focus on one tool
* Miss another task
* Execute incompletely

---

# Why ReAct Agent Performs Better?

## Answer

ReAct agents combine:

* Reasoning
* Action
* Observation loops

This enables:

* Multi-step planning
* Better tool orchestration
* Iterative reasoning

---

## ReAct Workflow

```text id="k4rvvk"
Thought -> Action -> Observation -> Thought
```

---

## Why better for multiple tools?

### Answer

Because ReAct agents:

* Continuously reason after each tool call
* Decide next actions dynamically
* Handle complex multi-tool tasks better

---

# Functional API

## What is Functional API?

### Answer

Functional API defines workflows using functions instead of explicit node-edge graph construction.

---

## Features

* Simpler syntax
* Faster prototyping
* Less boilerplate

---

# Graph API vs Functional API

| Feature            | Graph API            | Functional API       |
| ------------------ | -------------------- | -------------------- |
| Structure          | Explicit nodes/edges | Function-based       |
| Flexibility        | High                 | Moderate             |
| Visualization      | Easier               | Harder               |
| Complex workflows  | Better               | Limited              |
| Production systems | Preferred            | Good for simple apps |

---

## Best Interview Answer

### When to use Graph API?

Use for:

* Complex workflows
* Multi-agent systems
* Conditional routing

### When to use Functional API?

Use for:

* Simple workflows
* Rapid prototyping
* Smaller applications

---

# ReAct Agent

## What is ReAct?

### Answer

ReAct stands for:

```text id="y53nxx"
Reason + Act
```

It combines reasoning traces with tool execution.

---

## Why important?

### Answer

ReAct enables:

* Dynamic planning
* Multi-step reasoning
* Better tool usage
* Iterative decision making

---

## Interview One-Liner

> "ReAct agents iteratively reason and act, making them more effective for complex multi-tool workflows."

---

-----------------------------------------------------------------------------------------

## ReAct agent architecture:
    - 1. Act:
    - 2. Observe:
    - 3. Reason:

---

## Persistent Checkpointing

## `config={"configurable":{"thread_id":"1"}}` - Importance of this


## Steaming techniques in LangGraph

## Streaming 
Methods: .stream() and astream()

- These methods are sync and async methods for streaming back results.

Additional parameters in streaming modes for graph state

- **values** : This streams the full state of the graph after each node is called.
- **updates** : This streams updates to the state of the graph after each node is called.

## Compare Values and Updates mode and when to use which?

## Use of async and interpret why it gives detailed output?
`config = {"configurable": {"thread_id": "5"}}

async for event in graph_builder.astream_events({"messages":["Hi My name is Rohan and I like to play cricket"]},config,version="v2"):
    print(event)`

## Human In the Loop
    - Command: 
    - Interrupt: 