# Context engineering and retrieval

Agent systems and standards · Chapter 6 of 8 · 25 minutes

Choosing what each model call reads: grep or vector search, offloading large results, a first look at subagents, and memory that lasts across threads.

## Goals

- Run grep and vector search for the same words and compare.
- Let a large tool result be offloaded instead of filling the context.
- Hand a search to subagents and compare the context sizes.
- Pass the check.

## The read

### What each call reads

A model call reads its context: the system prompt, the messages so far, and every tool result. Context engineering is deciding what goes in. Too little and the agent guesses; too much and it costs more, runs slower and gets distracted by text that does not matter, or that was planted.

### grep or vectors

grep finds exact text, line by line: an order number, a ticket id, a date. It costs nothing and misses anything worded differently.

Vector search finds similar meaning. Each chunk of each file is turned into a vector (here, 384 numbers from the all-MiniLM-L6-v2 embedding model), and chunks are ranked by cosine similarity to the query's vector. It finds "payment taken two times" for "charged twice", but for 48213 it ranks tickets about orders 48144 and 48231 ahead of the right one. Filters such as a folder or a date keep stale or unrelated chunks out.

### Offload, skills and subagents

A tool can return far more than a context can hold. Eviction saves a large result to a file and gives the model the path and a short preview; the agent then greps the file and reads only the lines it needs.

Skills follow the same idea: the agent sees each skill's name and a one-line description, and reads its full instructions (a SKILL.md file) only when a task needs them.

A subagent gets a fresh context with only its task and returns one report, so its reading stays out of the main context. Advanced agents, chapters 9 and 10, cover subagents, thresholds and compaction in depth.

### Memory across threads

A thread is one conversation: its checkpointer keeps that thread's messages, and a new thread starts empty. Long-term memory is a store that every thread shares. LangGraph's InMemoryStore keeps items under a namespace, such as customers / mara-lind; one thread puts a note there, and a later thread searches for it. In production the store is a database.

### In this lab

Halden's support agent works ticket T-48213. Its harness re-creates Deep Agents (a LangChain open-source harness) on LangGraph.js: filesystem tools, eviction over 20,000 tokens, and task subagents. The memory step uses LangGraph.js's own InMemoryStore.

## Try it: What the agent reads

Search for the same words with grep and with vectors, let the payments export be offloaded, hand the reading to subagents, then save a note that a later thread reads back.

https://agenticademy.org/en/paths/agent-systems/06-context-engineering

## Check

### Question 1 of 4

The agent must find every ticket that mentions order 48213. Which search fits best?

- Vector search.
- **✓** grep for 48213.
- Ask the model to remember it.

**Why:** An exact id is what grep is for. Vector search ranks by meaning, and in this lab it ranks tickets about orders 48144 and 48231 ahead of the right one.

### Question 2 of 4

A tool returns a 40,000-row export. What does eviction put in the context?

- All 40,000 rows.
- **✓** The file's path and a short preview; the agent reads pages of it when needed.
- Nothing at all.

**Why:** The full result is saved to a file, so the context stays small and the agent can still grep and page through the data.

### Question 3 of 4

What comes back from a subagent to the agent that started it?

- Its whole conversation.
- **✓** One final report of its work.
- Its tools.

**Why:** The subagent's reading stays in its own context. Only its report joins the parent's context, which keeps the parent small and keeps planted text out of it.

### Question 4 of 4

Thread 1 saved a note in the store. Thread 2 starts with no messages. Can it read the note?

- No: a new thread starts empty.
- **✓** Yes: every thread shares the store; only the messages belong to one thread.
- Only if thread 2 copies thread 1's messages.

**Why:** The checkpointer keeps each thread's messages apart. The store sits outside the threads, so any thread can search the customer's namespace.

## What you can do now

- Choose between grep and vector search for a query, and say when a folder or date filter helps.
- Offload large tool results and page through them.
- Use subagents to keep a main agent's context small and clean.
- Save a note in a long-term store and read it back in a later thread.

---

[Back to Agent systems and standards](README.md) · [Next: Observability: traces and runs](07-observability.md)
