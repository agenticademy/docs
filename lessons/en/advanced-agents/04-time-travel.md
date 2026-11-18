# Time travel

Advanced agents · Chapter 4 of 10 · 20 minutes

Every superstep saves a checkpoint, so you can read a run's history, replay from a past step, edit a past state and fork a second ending.

## Goals

- Run a five-node graph on a thread and read its checkpoint history.
- Rewind to step 3 and replay from it.
- Edit the state at step 3, fork, and compare both endings.
- Put the steps of a fork in order.

## The read

### A thread is a timeline

Compile a graph with a checkpointer, run it with a thread\_id, and LangGraph saves a checkpoint of the whole state after every superstep, as in the Agent systems path. Each checkpoint has its own checkpoint\_id, a step and a source in its metadata (step -1 is the input), and a parentConfig that names the checkpoint before it. Linked by their parents, a thread's checkpoints form a timeline, and a timeline can branch.

### Read the past

getStateHistory({ configurable: { thread\_id } }) lists every checkpoint of the thread, newest first. Each item is a state snapshot: values, next (the nodes due to run), config and metadata. getState with a checkpoint\_id in its config reads one of them. That is rewinding: you look at a past state and change nothing.

### Replay and fork

invoke(null, config), with a config that names a past checkpoint, replays from it. Nodes before that checkpoint do not run again; the nodes after it do, so a real model may answer differently the second time. The new checkpoints grow on a new branch that starts with a copy whose source is fork. The old ones stay.

updateState(config, values, asNode) changes the past without erasing it. It writes a new checkpoint, source update, on top of the one config names, as if the node asNode had returned values, and returns the new checkpoint's config. Run invoke(null, thatConfig) and the thread forks: the first ending stays in the history and a second one grows from your edit. getState without a checkpoint\_id reads the newest checkpoint, which is now the end of the fork.

### Durability

A run's durability option decides when checkpoints are written. "async", the default, saves each one while the next superstep runs; "sync" saves it before the next superstep starts; "exit" saves only when the run ends. "exit" writes the least, but it leaves no past step to rewind to, and nothing to resume from if the process crashes halfway.

### In this lab

Halden's refund check for ticket T-48213 runs five nodes in a row: read\_ticket, classify, list\_charges, decide and draft\_reply. LangGraph.js 1.5.1 and its MemorySaver run in this page, and classify, decide and draft\_reply ask a scripted model. The browser build of LangGraph.js has no AsyncLocalStorage, so each node passes its config to the model explicitly.

## Try it: A checkpoint rail

Three steps: run the graph and read its checkpoints, rewind to step 3 and replay, then edit step 3, fork, and compare both endings.

https://agenticademy.org/en/paths/advanced-agents/04-time-travel

## Check

Put the steps of forking a run from a past checkpoint in order.

1. Run the graph with a checkpointer and a thread\_id.
2. List the thread's checkpoints with getStateHistory.
3. Pick the checkpoint to go back to by its checkpoint\_id.
4. Call updateState on that checkpoint's config with the new values.
5. Call invoke(null, config) with the config updateState returned.
6. Compare the fork's ending with the first one, still in the history.

**Why:** Checkpoints exist only once a run on a thread has saved them, and you find a past one in getStateHistory by its checkpoint\_id. updateState needs that checkpoint's config and returns the config of the new checkpoint it writes; invoke(null, …) with that config runs the fork. Nothing is overwritten, so the first ending is still there to compare.

## What you can do now

- Read a thread's checkpoints with getStateHistory, and one of them with getState and its checkpoint\_id.
- Replay a run from a past checkpoint, and say which nodes run again.
- Edit a past state with updateState, fork the thread, and compare both endings.
- Choose a durability mode, knowing what each one leaves you to rewind to.

---

[Back to Advanced agents](README.md) · [Next: Loops that survive](05-loops-that-survive.md)
