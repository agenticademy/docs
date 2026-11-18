# Memory and context

Agents for everyone · Chapter 4 of 8 · 12 minutes

What an agent can see at each step is limited, so what it keeps, drops and writes down decides what it knows.

## Goals

- Fill the context until the earliest messages drop out.
- Save the right fact so the agent answers correctly later.
- Pass the check.

## The read

### What the model reads

Each time the model decides a step, it reads one block of text called the context: your request, the conversation so far and what the tools returned. That is all it has. If something is not in the context, the model does not know it for that step.

### A desk with limited room

The context has a size limit, counted in tokens. A token is a piece of a word: a short word is about one token, and in English one token is about four characters. Limits differ between models, from a few thousand tokens to a million or more, but there is always one.

Think of the context as a desk. Only so much paper fits on it at once.

### When the desk is full

A long conversation fills the desk. Then something has to go: the oldest messages are dropped, or several are replaced by a short summary. Whatever is dropped, the agent no longer knows, even if you told it earlier.

### Memory: a notebook beside the desk

Memory is something different. It is notes the agent writes somewhere outside the context, such as a file, and reads back when they are needed. A note survives when the desk is cleared, but only if the agent saved it, and only the part it chose to write down.

### More is not always better

A crowded context takes longer to read, costs more and can bury the one fact that matters. Good agents keep on the desk what the next step needs, and write the rest in the notebook.

## Try it: A desk with limited room

Fill the desk until the oldest messages drop off, and see what the agent forgets. Then start again, save one fact in the notebook and ask again.

https://agenticademy.org/en/paths/agents-for-everyone/04-memory-and-context

## Check

### Question 1 of 3

What is in an agent's context for one step?

- Everything on the internet.
- **✓** The request, the conversation so far and tool results that still fit.
- Only your last message.

**Why:** The model reads only what is placed in front of it for that step, up to its size limit.

### Question 2 of 3

The context is full and a new message arrives. What usually happens?

- The agent stops working for good.
- **✓** Older messages are dropped or summarised to make room.
- The model grows a larger context.

**Why:** The limit is fixed for a model, so the agent has to make room. What it drops it no longer knows, unless it saved a note.

### Question 3 of 3

How is memory different from context?

- **✓** Memory is notes kept outside the context and read back when needed.
- They are the same thing.
- Memory is what the model learned in training.

**Why:** Memory survives when the context is cleared, because it lives elsewhere. The agent has to choose to save it and to read it back.

## What you can do now

- Explain what a context is and why it has a limit.
- Say what happens when it fills up.
- Tell context from memory, and say why saving the right facts matters.

---

[Back to Agents for everyone](README.md) · [Next: Plans, and agents working together](05-plans-and-teams.md)
