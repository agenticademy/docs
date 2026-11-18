# Subgraphs and teams

Advanced agents · Chapter 3 of 10 · 20 minutes

Build teams of agents from subgraphs, decide how they share state, read their namespaces in the stream, and compare multi-agent patterns by model calls and tokens.

## Goals

- Run a team of teams and read the namespaces in its stream.
- Pause inside a team and open its own state.
- Compare subagents with hand-offs on model calls and tokens.
- Predict what a subgraph streams.

## The read

### Subgraphs

A compiled graph can be a node of another graph: a subgraph. When the two share a state key, the compiled subgraph is the node, as in addNode("research\_team", researchTeam). It reads the keys it shares with the parent, what it writes to them comes back, and keys only it has stay inside.

With no shared key, call the subgraph from a wrapper node that maps the parent's state to the subgraph's input, and its output back to an update. Pass { subgraphs: [team] } to addNode so getGraphAsync and getState can still find it.

### Namespaces and streaming

Each subgraph run saves its checkpoints in its own namespace inside the parent's thread, named after its node and task: research\_team:<task id>. stream(input, { streamMode: "updates", subgraphs: true }) yields [namespace, update] pairs, with [] for the top graph. getGraphAsync({ xray: true }) draws the subgraphs' own nodes, such as research\_team:lead.

While a run is paused inside a subgraph, getState(config, { subgraphs: true }) also returns the subgraph's own state. In a browser interrupt() cannot run, because it needs AsyncLocalStorage, so this lab pauses with interruptBefore and passes config to each subgraph by hand.

### Teams of teams and hand-offs

A supervisor can manage other supervisors. In the lab a head of support routes with Command({ goto }) to two teams, and each team's lead hands parts of the job to its workers. A node inside a subgraph can also pass control to a node of the parent: new Command({ graph: Command.PARENT, goto: "writer", update }). That is a hand-off: the next agent takes over with the history you put in update.

### Choosing a pattern

LangChain's JS docs name five multi-agent patterns. Subagents: a main agent calls other agents as tools and gets each result back. Hand-offs: the agent in charge changes. Skills: one agent loads specialist prompts when it needs them. Router: a step classifies the input and sends it to agents. Custom workflow: your own LangGraph graph. Agent systems chapter 5 named its transfer\_to\_<agent> calls hand-offs, but its supervisor used subagents: each result came back to it.

The docs compare them (read 2026-11-04): a one-shot request takes 4 model calls with subagents and 3 with hand-offs; a question across three domains takes 5 calls and about 9K tokens with subagents, 7+ calls and about 14K+ with hand-offs. In the lab you run one job both ways and count.

## Try it: A team of teams

Three steps: run a team of teams and read its stream, pause inside a team to open its state, then run the same job as subagents and as hand-offs.

https://agenticademy.org/en/paths/advanced-agents/03-subgraphs-and-teams

## Check

The team shares messages with the top graph; notes is its own key. What does the loop print?

Code:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"], then top team ["messages"]: the team's own update first, then what comes back to the top graph.
- top team ["notes", "messages"]: the stream shows the top graph only, and the subgraph's keys come back with it.
- inside work ["notes", "messages"], then top team ["notes", "messages"]: every key the team wrote comes back.
- It throws: the top graph has no notes key, so the team cannot write one.

**Why:** With subgraphs: true the stream also yields the subgraph's own updates, under its namespace ["team:<task id>"]; without it you would see only the top line. The team is the node itself, so the keys it shares come back: messages. notes exists only inside the team, so it stays there, and writing it there is allowed.

## What you can do now

- Add a subgraph as a node when it shares keys, or call it from a wrapper node that maps the state.
- Stream with subgraphs: true and read each update's namespace; read a paused subgraph's state with getState.
- Hand control to a parent graph's node with Command.PARENT, and build supervisors of supervisors.
- Choose between subagents, hand-offs, skills, a router and a custom workflow by their model calls and tokens.

---

[Back to Advanced agents](README.md) · [Next: Time travel](04-time-travel.md)
