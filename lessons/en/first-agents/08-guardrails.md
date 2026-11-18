# Guardrails

Build your first agents · Chapter 8 of 8 · 22 minutes

Checks in code around the model: input and output checks, tool allow-lists and step caps, in layers.

## Goals

- Turn on each guard and see what it stops.
- Stop all four attacks in one replay.
- Predict what the step cap does.

## The read

### Rules the model cannot talk its way out of

A guardrail is a check in your code, outside the model. A prompt can ask the model to follow a rule, and a clever message can talk it out of that rule. Code that refuses a call cannot be talked out of anything.

### Four kinds of check

Input checks test the user's message before the model reads it, and refuse or flag it. Output checks test the answer before anyone sees it: for example, every amount it states must appear in a tool result, so the model cannot invent one.

Allow-lists limit what can happen: which tools may run, which hosts they may reach, which amounts they may move. They deny by default, so a tool or an argument nobody listed is refused, even when an instruction hidden in a ticket asks for it. Put the allow-list around the tools node, so it refuses before the tool runs.

Step caps stop runaway loops. recursionLimit in the run's config caps the supersteps, and LangGraph throws GraphRecursionError when it is reached. Catch it by err.name and turn it into a clear stop. A budget on tokens or model calls works the same way.

### In layers

No single check catches everything. The input check never sees text that arrives in a tool result; the allow-list does not read answers. So you stack them, and when one layer misses, the next one can still stop the attack.

Dedicated safety models are another layer: classifiers such as NVIDIA's Nemotron content-safety models and the Llama Nemotron safety guard read a message and label it safe or unsafe. They catch what a pattern misses, but they are models too, so keep the code checks as well.

## Try it: Four guards, four attacks

Fire four attacks at an agent with no guards. Then turn on one guard per attack and see what it stops. End with one replay that stops all four.

https://agenticademy.org/en/paths/first-agents/08-guardrails

## Check

The scripted model always asks for another tool call. What happens?

Code:

```
await agent.invoke(
  { messages: [new HumanMessage("Check ticket T-48213")] },
  { recursionLimit: 6 },
);
```

- It runs forever.
- **✓** It throws GraphRecursionError after 6 steps.
- It returns the last message after 6 steps, without an error.
- recursionLimit only applies to subgraphs.

**Why:** recursionLimit caps the number of supersteps. When it is reached LangGraph throws GraphRecursionError, which your code catches and turns into a clear stop.

## What you can do now

- Write input and output checks around a model.
- Wrap a tool node with an allow-list that denies by default.
- Cap an agent's steps and handle GraphRecursionError.
- Explain why guardrails come in layers.

---

[Back to Build your first agents](README.md)
