# Loops that stop

Advanced agents · Chapter 6 of 10 · 20 minutes

A loop needs stops that hold whatever the model does: a recursion limit, a budget of calls, a detector for repeated calls, and a person who can say no.

## Goals

- Run an agent that repeats one tool call until GraphRecursionError stops it.
- Arm a call budget and a repeat detector, and see which fires first.
- Pause before a risky tool and decline the call.
- Pass the check.

## The read

### A loop needs stops the model does not choose

An agent loop ends when the model answers without asking for a tool. A turn is one model call plus the tools it asks for. If the model keeps asking, for example to check a status that never changes, every turn is another paid model call and nothing new is learned. So you add stops that hold whatever the model does.

### LangGraph's cap: recursionLimit

In a loop like this one, each node run is one superstep. recursionLimit in the run's config caps the supersteps, 25 by default. Past it, LangGraph.js throws GraphRecursionError, with lc\_error\_code GRAPH\_RECURSION\_LIMIT. It is a backstop: it counts supersteps, not turns, it stops late, and without a checkpointer the run's state is lost with the error.

### Stops as middleware

Middleware are hooks that LangChain 1.6's createAgent runs around the model call; beforeModel and afterModel hooks each become a node of the graph. modelCallLimitMiddleware is a budget in model calls: runLimit counts one run, threadLimit a whole thread, and exitBehavior 'end' ends the run with a message. toolCallLimitMiddleware counts calls to one tool or to all. A budget in tokens or money works the same way: count, compare, stop. This site re-creates both, as modelCallLimit and toolCallLimit; the lab uses modelCallLimit.

A repeat detector catches a stuck loop early: the same tool with the same arguments, threshold times in the last few calls. The repeatDetector here is this site's own; LangChain has no built-in for it. With exitBehavior 'end' it ends the run; with 'continue' it refuses the call and tells the model to try something else. Here each hook also writes a trace event, so you can see which stop fired.

### A person as the stop

Some calls should wait for a yes, such as sending money. interruptBefore pauses the graph before a node, and a checkpointer keeps the paused state. The breakpoint stops before every run of that node, so your code picks the calls that need a person and shows them. To decline, updateState answers the call with a ToolMessage as if the tools node had run; invoking again with a null input resumes the run. In Node, humanInTheLoopMiddleware does this with interrupt(), which needs AsyncLocalStorage; browsers lack it, so this page uses the breakpoint.

## Try it: Three ways to stop a loop

A scripted agent checks a refund that stays pending. Stop it with recursionLimit, then with a budget and a repeat detector, then with a person before the risky tool.

https://agenticademy.org/en/paths/advanced-agents/06-loops-that-stop

## Check

The model asks for check\_refund({ order: '4471' }) on every turn and never answers. Which stop ends this run?

Code:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError, at superstep 25.
- The repeat detector, on the fourth identical call.
- **✓** The call budget, before the fourth model call.
- None: a run only ends when the model answers.

**Why:** The call budget allows three model calls; before the fourth, modelCallLimit ends the run. So the repeat detector never sees a fourth identical call. Each turn here takes five supersteps, so the run ends at superstep 16, well before recursionLimit's 25. And a stop never waits for the model: that is the point of a stop.

## What you can do now

- Set recursionLimit as a backstop and catch GraphRecursionError.
- Add a call budget and a repeat detector as middleware, and read which one fired from its trace event.
- Pause before a risky tool with interruptBefore, and decline the call with updateState.

---

[Back to Advanced agents](README.md) · [Next: Planning loops](07-planning-loops.md)
