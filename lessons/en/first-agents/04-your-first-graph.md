# Your first graph

Build your first agents · Chapter 4 of 8 · 20 minutes

Building a StateGraph from nodes and edges, routing with a conditional edge, and running it step by step.

## Goals

- Connect START, classify, both desks and END into a graph that compiles.
- Run the graph so that each branch is taken once.
- Predict how the graph runs.

## The read

### Nodes and edges

A StateGraph starts from a state definition, such as Annotation.Root({ … }) or the MessagesAnnotation of chapter 3. addNode(name, fn) adds a node: a function that reads the state and returns an update, which the state's reducers merge in. addEdge(a, b) says that b runs after a. START and END are the graph's entry and exit: an edge from START picks the first node, and an edge to END finishes the run.

### Branches

addConditionalEdges(from, router, targets) puts a decision after a node. The router is a function of the state that returns the name of the next node, and targets lists the nodes it may return. This is how a graph routes a refund request to one desk and a question to another. Without a router, two plain edges out of one node both run.

### Compile, then run

compile() checks the structure and returns a graph you can invoke() or stream(). It throws if something is wrong, for example a node that no edge reaches (UnreachableNodeError). It does not require every node to reach END: a node with no outgoing edge simply ends the run there.

A run goes in supersteps. In each one, every node that is ready runs, all together; then their updates are merged into the state, and the edges decide which nodes are ready next. stream() reports each node's update as it happens. If two nodes in one superstep write the same key, that key needs a reducer to combine them, or LangGraph raises an error.

## Try it: Wire a graph

Draw the five edges from START to END; LangGraph compiles the graph after each one. Then run both sample tickets and watch each superstep.

https://agenticademy.org/en/paths/first-agents/04-your-first-graph

## Check

How does this graph run?

Code:

```
new StateGraph(State)
  .addNode("a", a).addNode("b", b).addNode("c", c)
  .addEdge(START, "a")
  .addEdge("a", "b").addEdge("a", "c")
  .addEdge("b", END).addEdge("c", END)
  .compile();
```

- a, then b, then c: three supersteps.
- **✓** a, then b and c together in one superstep: two supersteps.
- a, b and c all at once.
- It does not compile: a node cannot have two outgoing edges.

**Why:** After a finishes, both b and c are ready, so they run in the same superstep and their updates merge.

## What you can do now

- Build and compile a StateGraph with nodes, edges, START and END.
- Route with addConditionalEdges.
- Explain supersteps and when nodes run in parallel.

---

[Back to Build your first agents](README.md) · [Next: Tools and tool calls](05-tools-and-tool-calls.md)
