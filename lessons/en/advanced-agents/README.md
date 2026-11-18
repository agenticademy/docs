# Advanced agents

Advanced · For developers · 10 chapters · 205 min

For developers who can already build agent systems. You work with large graphs and their state, route with Command and Send, rewind and fork runs, build loops that recover from failure and know when to stop, compare planning patterns, and run a deep-agent harness on a long job. Each lab runs real LangGraph.js 1.5.1 in your browser with a scripted model. The deep-agent harness is re-created, and the screens say so.

## Before you start

The Agent systems path, or the same knowledge: graphs, tools, the agent loop, checkpoints and traces.

## By the end you can

- Design graph state with reducers and Overwrite, and predict how parallel writes combine.
- Route with Command and Send, and build teams from subgraphs.
- Rewind, edit and fork a run from its checkpoints.
- Build loops that retry, compensate, resume after a crash, and stop on limits, budgets or a person's word.
- Choose a planning or compute-spending pattern, and run a deep-agent harness on a long job.
- Carry a long job across sessions with hand-off files, compaction and a Store, and grade its trajectory with rules in code.

## Chapters

1. [State at scale](01-state-at-scale.md): How LangGraph.js 1.5.1 merges writes to one state key: StateSchema fields with and without reducers, the order of parallel writes, Overwrite and UntrackedValue.
2. [Command and Send](02-command-and-send.md): A node can update state and choose the next node in one step with Command, and fan work out to parallel workers with Send, as many as the model asks for.
3. [Subgraphs and teams](03-subgraphs-and-teams.md): Build teams of agents from subgraphs, decide how they share state, read their namespaces in the stream, and compare multi-agent patterns by model calls and tokens.
4. [Time travel](04-time-travel.md): Every superstep saves a checkpoint, so you can read a run's history, replay from a past step, edit a past state and fork a second ending.
5. [Loops that survive](05-loops-that-survive.md): Retry policies, timeouts, error handlers and checkpoints keep a long run going when a tool fails or the process stops.
6. [Loops that stop](06-loops-that-stop.md): A loop needs stops that hold whatever the model does: a recursion limit, a budget of calls, a detector for repeated calls, and a person who can say no.
7. [Planning loops](07-planning-loops.md): Plan-and-execute, ReWOO and LLMCompiler plan the work before they do it, and they differ in model calls, supersteps and time.
8. [Spending compute](08-spending-compute.md): Evaluator-optimizer loops, reflection, best-of-n and tree search spend more model calls to get a better answer, and each needs a budget.
9. [A deep-agent harness](09-deep-agent-harness.md): A deep agent plans with a todo list, keeps notes in virtual files, hands parts of a job to subagents with a fresh context, and pauses before the tools you name.
10. [Long-horizon agents](10-long-horizon.md): A job that spans sessions needs compaction inside a session, hand-off files and memory between sessions, and a check of its whole trajectory.

https://agenticademy.org/en/paths/advanced-agents
