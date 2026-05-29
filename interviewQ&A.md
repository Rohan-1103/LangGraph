# Most Important LangGraph Interview Questions

## 1. What are the core components of LangGraph?

### Answer

Nodes, edges, and state.

---

## 2. Why is `Annotated` used with state?

### Answer

To attach reducers like `add_messages` for controlled state updates.

---

## 3. What does `add_messages` do?

### Answer

It appends new messages to existing state instead of overwriting them.

---

## 4. Why is StateGraph important?

### Answer

It enables stateful multi-step workflow execution.

---

## 5. What are conditional edges?

### Answer

Edges that dynamically decide the next node based on runtime conditions.

---

## 6. Why are ReAct agents better than simple StateGraphs for multiple tool calls?

### Answer

Because ReAct agents iteratively reason after every tool execution and dynamically decide the next action.

---

## 7. Difference between Graph API and Functional API?

### Answer

Graph API provides explicit workflow control, while Functional API offers simpler function-based workflow definitions.

---

## 8. Why use reducers in LangGraph?

### Answer

Reducers control how multiple state updates merge without overwriting data.

---

## 9. What is the purpose of streaming in LangGraph?

### Answer

Streaming provides incremental outputs in real-time during graph execution.

---

## 10. What is shared state in LangGraph?

### Answer

Shared state is the central memory passed between nodes during execution.

---

# Best Interview One-Liner

> "LangGraph enables stateful, controllable, and dynamic AI workflows using nodes, edges, reducers, and shared state management."