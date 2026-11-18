# Long-horizon agents

Advanced agents · Chapter 10 of 10 · 25 minutes

A job that spans sessions needs compaction inside a session, hand-off files and memory between sessions, and a check of its whole trajectory.

## Goals

- Run session 1 and read the files it leaves for the next session.
- Start session 2 from the files alone, and watch compaction replace older messages with a summary.
- Grade the coding sessions' tool calls, and open a rule that session 3 broke.
- Pass the check.

## The read

### Longer than one context

A long-horizon job needs more model calls than one context window holds, or more than one session: building an app feature by feature, say. The context window is the most a model reads in one call, counted in tokens. A session here is one run of the agent on a new thread. Two things keep such a job on track: shrinking the context inside a session, and handing the work over between sessions.

### Offloading and compaction

LangChain's post on context management for Deep Agents (2026-01-28) describes three layers. A tool result over 20,000 tokens goes to a file, and the agent gets its path and a preview. Past 85% of the window, the inputs of old file writes and edits become pointers to their files. When that is not enough, compaction: a model writes a summary of the intent, the artifacts and the next steps, the summary replaces the older messages, and the full transcript stays on disk. This lab's re-created harness offloads at 20,000 tokens and compacts at 85% of the window (summarize: { window, fraction, keep }), with no middle layer.

### Hand-off files and memory

Anthropic's post on harnesses for long-running agents (2025-11-26) starts each job with an initializer session. It writes a JSON feature list with every feature failing, a progress file, an init.sh script and a first git commit. Every later session reads the progress file, the git log and the feature list, runs a basic end-to-end test, works on one feature, sets its passes field to true only after testing it, and ends with a commit and a progress note. Nothing else in the list changes. For facts every thread needs, LangGraph's Store keeps long-term memory under a namespace, outside any thread.

### Reset or compaction

Starting a session from these files is a context reset: a clean context and a written hand-off. Compaction keeps one context and shrinks it. In Anthropic's post of 2026-03-24, Sonnet 4.5 wrapped up early as its context filled, so that harness reset; with Opus 4.5 it switched to compaction.

### Grade the trajectory

A trajectory is the sequence of tool calls a run made. An answer can say "done" when no test ran, so grade the calls with rules in code, like the code checks in Agent systems, chapter 8. Read the calls from the trace: after compaction, the messages no longer hold the early ones.

## Try it: Three sessions, one job

Run session 1, then session 2 from its files alone, then session 3, and grade the coding sessions' trajectories.

https://agenticademy.org/en/paths/advanced-agents/10-long-horizon

## Check

A coding session of a long job starts on a new thread. Put its steps in order, as the harness in Anthropic's post runs them.

1. Read progress.txt, the git log and feature\_list.json.
2. Run the smoke test to check that the app still works.
3. Write the code for one feature.
4. Run that feature's tests.
5. Set that feature's passes to true in feature\_list.json.
6. Add a line to progress.txt and commit.

**Why:** The new thread knows only what the files say, so it reads them first. The smoke test catches anything the last session left broken before new code goes on top. A feature passes only once its own tests pass on the new code: setting passes first is the shortcut the checker flagged in session 3. The progress line and the commit are what the next session reads.

## What you can do now

- Set up a long job with a feature list, a progress file and commits, so each session can start from the files.
- Compact a context with summarize at a threshold, and offload large tool results to files.
- Keep long-term memory in a LangGraph Store that every thread reads.
- Grade a run by its tool calls, with rules in code read from the trace.

---

[Back to Advanced agents](README.md)
