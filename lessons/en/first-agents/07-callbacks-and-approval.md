# Callbacks and human approval

Build your first agents · Chapter 7 of 8 · 22 minutes

Watching a run with callbacks, and pausing it before a risky tool so a person can approve or reject it.

## Goals

- Attach a callback handler and watch its events.
- Pause before the refund tool, then approve it.
- Reject a refund and see the agent handle it.
- Pass the check.

## The read

### Callbacks watch

A callback handler is an object whose methods LangChain calls as a run goes: a subclass of BaseCallbackHandler with methods such as handleChatModelStart, handleLLMEnd, handleToolStart and handleToolEnd. You pass it in config.callbacks, and it reaches every nested call that receives that config. Use it for logs, metrics and traces. A callback only observes: LangChain ignores what its methods return and, by default, only logs an error thrown inside one, so it cannot change or stop a step.

### Breakpoints pause

To stop a run before a step, LangGraph needs to save the state and come back to it. That is a checkpointer: compile({ checkpointer: new MemorySaver() }) saves a checkpoint after every superstep, under the thread\_id in config.configurable. Add interruptBefore: ["approval\_tools"] and the run stops before that node.

While it is paused, (await graph.getState(config)).next says what is waiting, and the last AIMessage holds the pending call. To approve, resume with graph.invoke(null, config): a null input means "go on from the checkpoint". To reject, call graph.updateState(config, { messages: [toolMessage] }, "approval\_tools") first: your ToolMessage answers the call as if the node had run, so the node is skipped and the model reads your refusal.

Give risky tools their own node, as here with create\_refund, so the gate stops only them.

### In Node, and elsewhere

In Node, the langchain package's createAgent offers middleware hooks (beforeModel, afterModel, wrapModelCall, wrapToolCall) and humanInTheLoopMiddleware, which pauses with interrupt(). interrupt() needs AsyncLocalStorage, so in a browser you use breakpoints, as here. Coding agents use the same idea: hooks that run before a tool call and can deny it.

## Try it: A gate before the refund

Four steps, one action each, and an optional fifth: watch a run's callback events, put a gate before the refund, approve one run, then reject the next.

https://agenticademy.org/en/paths/first-agents/07-callbacks-and-approval

## Check

### Question 1 of 3

What does interruptBefore need to work?

- **✓** A checkpointer, and a thread\_id in the config.
- A callback handler.
- A real model.

**Why:** Pausing means saving the state and coming back to it. The checkpointer stores it under the thread's id.

### Question 2 of 3

The run is paused before approval\_tools. How do you continue it as it was?

- Invoke again with the original input.
- **✓** graph.invoke(null, config) with the same thread\_id.
- Start a new thread.

**Why:** A null input tells LangGraph to resume from the saved checkpoint instead of starting a new run.

### Question 3 of 3

A callback handler's handleToolStart returns false when it sees create\_refund. What happens?

- The refund is blocked.
- **✓** Nothing changes: LangChain ignores the return value, so the refund runs. To stop a step, pause the graph or guard the tool.
- The run pauses before approval\_tools.

**Why:** Callbacks are for watching and logging. Control belongs in the graph: a breakpoint with updateState, as here, or a check inside the tool node.

## What you can do now

- Log a run's model and tool events with a callback handler.
- Pause a graph before a node with a checkpointer and interruptBefore.
- Resume or reject a pending step with getState, updateState and invoke(null).

---

[Back to Build your first agents](README.md) · [Next: Guardrails](08-guardrails.md)
