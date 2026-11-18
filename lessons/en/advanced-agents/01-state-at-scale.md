# State at scale

Advanced agents · Chapter 1 of 10 · 20 minutes

How LangGraph.js 1.5.1 merges writes to one state key: StateSchema fields with and without reducers, the order of parallel writes, Overwrite and UntrackedValue.

## Goals

- Run two parallel nodes that write the same key with no reducer, and read the error.
- Add a reducer and see how the two writes combine, in order.
- Use Overwrite to replace a reduced value.
- Predict the end of a superstep.

## The read

### One superstep, many writes

LangGraph runs a graph in supersteps, as in Agent systems, chapter 5. Every node that is due runs in the same superstep, in parallel, and returns an update: an object with the state keys it changes. When all of them have finished, LangGraph applies the updates and saves a checkpoint of the state. Nodes in one superstep never see each other's updates, so what matters is how two writes to the same key combine.

### StateSchema and reducers

Since LangGraph.js 1.1 you can declare state with StateSchema, one field per key. A field given as a plain schema, such as z.string() from Zod, holds the last value written and takes one write per superstep. Two writes to it in the same superstep fail with InvalidUpdateError, even when the values are equal, and no checkpoint is saved for that superstep.

A ReducedValue field has a reducer: a function (current, next) => value that LangGraph calls for each write, for example to append to a list. The older Annotation.Root API declares the same thing with Annotation({ reducer, default }).

### Which write goes first

A reducer gets a superstep's writes one at a time, in task-path order, not in the order the nodes finished. Task paths are compared as strings, and the path of a node started by an edge ends with its name, so here reviews goes before specs. Since 1.5.0, live runs and checkpointers use this same order, as LangGraph for Python does.

### Overwrite and UntrackedValue

new Overwrite(value), added in 1.2, skips the reducer: the key is set to value, for example to replace a long list with a summary. Other writes to that key in the same superstep are dropped, and two Overwrite writes to one key in one superstep fail.

An UntrackedValue field holds a value while the graph runs: nodes read it, but no checkpoint saves it, so a run resumed from a checkpoint starts without it. Use it for large or temporary data you can fetch again.

### The lab

Halden, a fictional outdoor shop, asks whether to stock a tent for winter. Two researcher nodes, reviews and specs, each add a note to notes in the same superstep; condense then sums the notes up. You run this graph three times: with no reducer on notes, with a reducer that appends, and with condense returning an Overwrite.

## Try it: A superstep timeline

Run the same graph three times: with no reducer, with a reducer, then with Overwrite. The timeline shows what each superstep wrote and what its checkpoint saved.

https://agenticademy.org/en/paths/advanced-agents/01-state-at-scale

## Check

reviews and specs both follow START, so they run in the same superstep. What happens when it ends?

Code:

```
const State = new StateSchema({
  topic: z.string(),
  notes: new ReducedValue(z.array(z.string()).default(() => []), {
    reducer: (current, next) => current.concat(next),
  }),
});

new StateGraph(State)
  .addNode("reviews", async () => ({ notes: ["reviews"], topic: "Ridge 2" }))
  .addNode("specs", async () => ({ notes: ["specs"], topic: "Ridge 2" }))
  .addEdge(START, "reviews")
  .addEdge(START, "specs");
```

- notes is ["reviews", "specs"] and topic is "Ridge 2".
- **✓** The run fails with InvalidUpdateError on topic.
- The run fails with InvalidUpdateError on notes.
- topic keeps the write of the node that finished last.

**Why:** topic is a plain field, so it takes one write per superstep, and it got two, even though both are "Ridge 2". notes has a reducer, so its two writes would merge, in task-path order. No field keeps whichever write finished last.

## What you can do now

- Declare state with StateSchema, choosing a plain field, a ReducedValue or an UntrackedValue for each key.
- Predict how parallel writes to one key combine, in which order, and when they fail.
- Use Overwrite to replace a reduced value.

---

[Back to Advanced agents](README.md) · [Next: Command and Send](02-command-and-send.md)
