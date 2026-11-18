# Orchestration: supervisors, fan-out, subgraphs

Agent systems and standards · Chapter 5 of 8 · 25 minutes

Patterns for several agents in one LangGraph graph: a supervisor that routes, Send for parallel work, subgraphs, and the checkpoint each superstep saves.

## Goals

- See two specialists run in the same superstep.
- Open a nested subgraph's run.
- Run the same job with one agent and with a supervisor.
- Predict the fan-out.

## The read

### Supersteps

LangGraph runs a graph in supersteps. In each superstep, every node that is due runs, in parallel; their updates are then merged into the state by each key's reducer, and the edges decide which nodes run next. Messages use a reducer that appends, so two nodes can both add messages in one superstep.

### Threads and checkpoints

Compile a graph with a checkpointer, such as MemorySaver, and pass a thread\_id when you run it. LangGraph saves a checkpoint of the whole state under that thread: one for the input, then one at the end of every superstep. getState({ configurable: { thread\_id } }) reads the latest one: the values, the step number and next, the nodes due to run. A later run on the same thread starts from that state.

### Supervisor, fan-out, subgraphs

A supervisor is an agent that decides which specialist works next. Here a hand-off is a tool call such as transfer\_to\_customer\_support, so the model chooses it like any other tool.

Fan-out runs work in parallel. A conditional edge can return a list of Send objects instead of node names. Each Send starts one run of a node with its own input, all in the next superstep, and the reducer merges what they return.

A subgraph is a compiled graph used inside a node. It has its own state and its own checkpoint namespace, and its runs nest under the node that called it. In this lab the Financial Analyst calls a Data Engineer subgraph of its own. Hand-offs with Command and subgraphs that share state come later, in the Advanced agents path.

### When one agent is enough

Each agent adds model calls, checkpoints and hand-offs, and one more place to fail. Split the work when its parts are independent, when one context would grow too large, or when a part needs different tools or instructions. Otherwise start with one agent.

In this lab, Halden's support team handles ticket T-48213, a customer charged twice for order 48213. Run it both ways and compare what the traces measure.

## Try it: A sky of agents

Three steps: run the job as a supervisor and watch each superstep save a checkpoint, open a subgraph in the run tree, then run the same job with one agent and compare.

https://agenticademy.org/en/paths/agent-systems/05-orchestration

## Check

results has a reducer that concatenates lists. What does the graph return?

Code:

```
new StateGraph(State)
  .addNode("lookup", s => ({ results: [s.id * 10] }))
  .addConditionalEdges(START, () => [new Send("lookup", { id: 1 }), new Send("lookup", { id: 2 })], ["lookup"])
  .addEdge("lookup", END)
  .compile()
  .invoke({});
```

- **✓** { results: [10, 20] }: two runs of lookup in one superstep.
- { results: [20] }: the second Send overwrites the first.
- { results: [10] }: only the first Send runs.
- It throws: a node cannot be sent to twice.

**Why:** Each Send starts its own run of lookup with its own input ({ id: 1 } and { id: 2 }). Both run in the same superstep, and the reducer concatenates their updates.

## What you can do now

- Build a supervisor that routes to specialist agents with hand-off tool calls.
- Fan out with Send and merge results with a reducer.
- Use a compiled graph as a subgraph, and say when one agent is enough.
- Run a graph on a thread and read its last checkpoint with getState.

---

[Back to Agent systems and standards](README.md) · [Next: Context engineering and retrieval](06-context-engineering.md)
