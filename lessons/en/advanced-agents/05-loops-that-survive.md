# Loops that survive

Advanced agents · Chapter 5 of 10 · 20 minutes

Retry policies, timeouts, error handlers and checkpoints keep a long run going when a tool fails or the process stops.

## Goals

- Inject a flaky tool and watch the retries.
- Route the failure to a compensation step.
- Stop the run midway and resume it from its checkpoint.
- Predict the attempts and the route.

## The read

### Retries and timeouts

A tool call can fail for a moment: a 503 from a busy service, or no reply at all. In LangGraph.js 1.5.1 a node can carry a retryPolicy: maxAttempts (the first attempt counts), initialInterval and backoffFactor for the waits between attempts, and retryOn to pick the errors worth another try. A timeout turns a hung call into a failed attempt. runTimeout caps an attempt's total time; idleTimeout caps the time without progress, such as a state write, a callback event or a heartbeat() call. Both throw NodeTimeoutError, which the retry policy treats like any other error.

### Error handlers and compensation

When the attempts run out, the node's errorHandler, added in LangGraph.js 1.4, runs instead of failing the run. It gets the node's input state and a NodeError, which holds the node's name and the error, and returns a state update or a Command({ goto }). Routing to compensation is the saga pattern: a step that undoes work done before the failure, such as releasing a room that was reserved but never paid for.

### Stop and resume

With a checkpointer, every superstep ends by saving a checkpoint on the thread. Pass a RunControl in the run's config and call requestDrain(): the nodes already running finish, the checkpoint is saved, and the run throws GraphDrained. Invoking again with null input on the same thread\_id starts at the next node; nodes that finished do not run again. With a checkpointer that saves to a database rather than to memory, this works from a new process too.

A crash gives no such warning. The node that was running loses its writes and runs again on resume, so a call with side effects should carry an idempotency key: a key the service remembers, so a repeated request gets the first result instead of acting twice.

### In this lab

A booking graph: the model plans the trip, then reserve, charge and confirm each call one service. Browsers have no AsyncLocalStorage, so interrupt() and the functional API's task() cannot run here. The lab uses the graph API and passes config to every call by hand. Its second process is a new graph on the same in-memory MemorySaver, and pulling the plug aborts the run's AbortSignal.

## Try it: Fail, recover, resume

Make a tool flaky and watch the retries, take the payment service down and follow the route to compensation, then stop the process and resume the thread.

https://agenticademy.org/en/paths/advanced-agents/05-loops-that-survive

## Check

charge\_card throws a 503 on every call. What happens when this runs?

Code:

```
const graph = new StateGraph(Trip)
  .addNode("reserve", reserve)
  .addNode("charge", charge, {
    retryPolicy: {
      maxAttempts: 2,
      initialInterval: 100,
    },
    errorHandler: () =>
      new Command({ goto: "release" }),
  })
  .addNode("release", release)
  .addEdge(START, "reserve")
  .addEdge("reserve", "charge")
  .addEdge("charge", END)
  .addEdge("release", END)
  .compile();

await graph.invoke(input);
```

- **✓** charge\_card is called twice; then the errorHandler sends the run to release, and invoke returns normally.
- charge\_card is called three times, the first try and two retries; then release runs.
- charge\_card is called twice; then invoke throws the 503.
- The errorHandler runs after the first 503, and then charge is tried again.

**Why:** maxAttempts counts every attempt, the first included, so charge\_card is called twice, not three times. The errorHandler runs only once those attempts are used up, never between them. Its Command sends the run to release, which ends at END, so invoke returns the state. A node with no errorHandler makes invoke throw the 503 once its attempts are used up.

## What you can do now

- Give a node a retryPolicy and a timeout, and predict how many attempts it makes.
- Route a node's last failure to a compensation step with errorHandler and Command.
- Stop a run with RunControl.requestDrain and resume it from its checkpoint without repeating finished nodes.
- Use idempotency keys so a node that runs again after a crash does not act twice.

---

[Back to Advanced agents](README.md) · [Next: Loops that stop](06-loops-that-stop.md)
