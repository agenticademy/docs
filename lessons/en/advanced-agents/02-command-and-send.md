# Command and Send

Advanced agents · Chapter 2 of 10 · 20 minutes

A node can update state and choose the next node in one step with Command, and fan work out to parallel workers with Send, as many as the model asks for.

## Goals

- Run a router node that updates state and jumps with Command.
- Fan out with Send to as many checks as the scripted model chooses, for two tickets.
- Resume the run: the reducer has gathered the results, and one node summarises them.
- Predict what the gathering node runs with.

## The read

### Command: update and route in one return

A node usually returns a state update, and the graph's edges decide what runs next. A node can return new Command({ update, goto }) instead. update is written to the state as a plain return would be, and goto names the next node, chosen while the node runs. A router can classify a ticket, store the class and jump to the right node in one step.

Declare where a Command may go with ends: addNode("triage", triage, { ends: ["plan", "reply"] }). compile() uses ends to check that every node can be reached: here plan and reply have no other way in, so without ends it throws UnreachableNodeError. getGraphAsync() draws ends as edges. A goto adds to the node's static edges; it does not replace them, so a node that returns Command usually has none.

### Send: as many workers as the model asks for

new Send("check", { ticket, check }) starts one run of a node, called a task, with its own input instead of the graph's state. A conditional edge that returns a list of Sends fans out: all those tasks run in the next superstep, at the same time. The list can come from the model, so the width is decided at run time: three checks for one ticket, five for another. Command's goto also accepts Sends.

### Gathering the results

The tasks all write the same key, so that key needs a reducer, as in the last chapter. At the end of the superstep the reducer merges their writes in task order, which is the order of the Sends, whatever order the tasks finished in. A node with an edge from the workers then runs once, in the next superstep, and reads them all. That is map-reduce in a graph.

### Hand-offs, and this lab

Inside a subgraph, new Command({ graph: Command.PARENT, goto }) jumps to a node of the parent graph. That is how one agent hands work to another; the next chapter builds teams with it.

In the lab, the ticket desk of Halden, the fictional shop from the last chapter, runs one graph: triage routes each ticket, plan lets the scripted model choose the checks, and summarise writes the answer. The page pauses runs with interruptAfter and interruptBefore, passed to stream(), because interrupt() needs AsyncLocalStorage, which the browser build of LangGraph.js does not have.

## Try it: Route, fan out, gather

Three steps on one LangGraph.js graph: route a ticket with Command, fan out to the checks the model picks, then resume the run to gather and summarise.

https://agenticademy.org/en/paths/advanced-agents/02-command-and-send

## Check

The scripted model chose three checks, and each check takes a different time. How often does summarise run, and what does it write?

Code:

```
// findings: a ReducedValue whose
// reducer concatenates lists.
// plan returns { checks:
//   ["order", "shipping", "stock"] }
new StateGraph(State)
  .addNode("plan", plan)
  .addNode("check", async ({ check }) =>
    ({ findings: [check] }))
  .addNode("summarise", (s) => ({
    answer: s.findings.join(" + "),
  }))
  .addEdge(START, "plan")
  .addConditionalEdges("plan", (s) =>
    s.checks.map((c) =>
      new Send("check", { check: c })),
    ["check"])
  .addEdge("check", "summarise")
  .addEdge("summarise", END)
  .compile();
```

- Three times, once per finished check: first one name, then two, then all three.
- **✓** Once, after all three: "order + shipping + stock", in the order of the Sends.
- Once, with all three names in the order the checks finished, so it can change from run to run.
- Once: "stock". Each check's write replaces the one before.

**Why:** The three Sends start three check tasks in the same superstep. When it ends, the reducer merges their writes in task order, which is the order of the Sends, not the order they finished. summarise has an edge from check, so it runs once, in the next superstep, and reads all three. Without the reducer, three writes to findings in one superstep would fail.

## What you can do now

- Route and update state in one step by returning Command from a node, with its targets declared in ends.
- Fan out with Send to a width the model chooses at run time.
- Gather parallel results with a reducer, and predict their order and when the next node runs.

---

[Back to Advanced agents](README.md) · [Next: Subgraphs and teams](03-subgraphs-and-teams.md)
