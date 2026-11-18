# Observability: traces and runs

Agent systems and standards · Chapter 7 of 8 · 20 minutes

Recording every model call, tool call and step as a tree of runs, and reading it to find what is slow or broken.

## Goals

- Find the slowest model or tool run in the trace.
- Find the failed tool run and its parent.
- Export the runs in LangSmith's format.
- Pass the check.

## The read

### Runs and traces

An agent's answer hides many steps. Tracing records each one as a run: a graph, a node, a model call or a tool call, with its inputs, outputs, start and end times, and any error. The runs behind one request form a tree, called a trace.

LangChain.js and LangGraph.js trace through callbacks. Pass a tracer such as LangChainTracer in the config's callbacks, and every runnable reports its start and end to it.

### LangSmith's run format

Each run has a run\_type, such as chain, llm or tool, an id, a trace\_id shared by the whole tree, and a parent\_run\_id. Its dotted\_order is a key built from the start time and id of each run from the root down to it, so sorting by it lays out the tree in the order things happened. Model runs carry usage\_metadata with token counts. LangGraph adds thread\_id, langgraph\_node and langgraph\_step to the metadata.

### Supersteps and the run tree

LangGraph runs a graph in supersteps. In each superstep the nodes that are due run, and their updates are applied together before the next superstep starts. langgraph\_step is that superstep's number, so one trace reads two ways: as a run tree, each run under its parent, and as a superstep timeline, one group of runs per step. The waterfall in the lab groups its rows by superstep.

### What to look for

Latency: which run is long, and is it the model or a tool. Errors: a failed tool run and the node that called it. Tokens: which calls read the most. Loops: the same step again and again.

Traces hold prompts, tool results and customer data, so treat them as sensitive. The LangSmith SDK can run an anonymizer on every run before it is sent. OpenTelemetry is the general standard for traces across services; LangSmith accepts OpenTelemetry traces too.

## Try it: The run waterfall

Five steps: run the agent and watch its trace grow, read its supersteps, find the slowest run and the failed tool, then export the runs.

https://agenticademy.org/en/paths/agent-systems/07-observability

## Check

### Question 1 of 5

What does dotted\_order give you?

- The cost of a run.
- **✓** A sortable key that places each run in its trace's tree, in time order.
- The model's name.

**Why:** It is built from the start time and id of each run from the root down to this one, so sorting runs by it rebuilds the tree in the order things happened, without following parent links one by one.

### Question 2 of 5

Two runs in a trace have the same langgraph\_step. What does that tell you?

- They were called by the same tool.
- **✓** They ran in the same superstep of the graph.
- They took the same time.

**Why:** langgraph\_step is the superstep's number. In each superstep LangGraph runs the nodes that are due and applies their updates together; every run inside those nodes carries that number.

### Question 3 of 5

What is a trace?

- A log file of errors.
- **✓** The tree of runs, model, tool and chain, behind one request.
- A screenshot of the app.

**Why:** One request becomes one trace; each step inside it is a run with its parent.

### Question 4 of 5

A tool run failed, but the agent still answered. Why is the parent run not marked failed?

- The tracer hides errors.
- **✓** The tools node caught the error and returned it to the model as a result, so the node itself finished.
- Parents never fail.

**Why:** An error is recorded on the run that threw it. Here the node turned it into an error ToolMessage, the model read it and retried.

### Question 5 of 5

Which should you avoid sending to a shared tracing service?

- Run timings.
- **✓** Customers' personal data inside prompts and tool results, unless you need it and are allowed to.
- Run types.

**Why:** Traces record inputs and outputs in full. Redact or limit what is captured before it leaves your system, for example with the SDK's anonymizer.

## What you can do now

- Read a superstep timeline and a run tree.
- Find the slow or failed run in a trace, and the run that called it.
- Name the main fields of LangSmith's run format.
- Redact personal data before a trace leaves your system.

---

[Back to Agent systems and standards](README.md) · [Next: Evaluation and safety](08-evaluation-and-safety.md)
