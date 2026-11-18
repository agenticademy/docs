# Tools: how an agent reaches the world

Agents for everyone · Chapter 3 of 8 · 12 minutes

Tools are how an agent looks things up and gets things done, and the model knows each tool only by its label.

## Goals

- Give the agent the right tool for three requests.
- See a vague tool description lead the agent to the wrong tool.
- Pass the check.

## The read

### What a tool is

A tool is a program an agent can use to do something it cannot do with text alone: check the weather, read a calendar, work out a sum, send an email. To use one, the agent fills in the details the tool needs (a city and a day, for a weather forecast) and reads what comes back.

### Tools that read, tools that change things

Some tools only read: they look something up and change nothing. Others write: they change something in the world, such as sending an email or buying a ticket. A mistake with a read tool costs a turn. A mistake with a write tool can cost much more, which is why careful agents ask you before they use one.

### The label is all the model sees

The model never sees inside a tool. For each tool it gets a label: a name, a short description of what the tool does, and the details it needs. It chooses by comparing your request with those labels.

So the words on the label matter as much as the tool itself. A clear label leads to the right choice. A vague one leads to mistakes, even when the right tool is right there.

### Fewer tools, fewer mistakes

Give an agent only the tools a job needs. Fewer choices mean fewer wrong picks, and an agent cannot misuse a tool it does not have.

Below, you play the agent's helper: hand it the right tool for three requests. Then change one label and see what the agent does with it.

## Try it: Pick the right tool

Hand the agent the right tool for three requests; pick a tool to read its label. Then let the agent pick from the labels, make Email's label vague, and send the request again.

https://agenticademy.org/en/paths/agents-for-everyone/03-tools

## Check

### Question 1 of 3

What does the model know about a tool when it decides to use it?

- How the tool's code works inside.
- **✓** Its name, its description and the details it needs.
- Everything the tool has ever returned.

**Why:** The label is all the model gets. That is why a clear description matters as much as the tool itself.

### Question 2 of 3

Which of these is a write tool, one that changes something?

- Check tomorrow's weather.
- Search for a restaurant.
- **✓** Send an email.

**Why:** Sending an email changes something outside the conversation and cannot be undone. Looking up weather or restaurants only reads.

### Question 3 of 3

Why give an agent only the tools a job needs?

- It makes the agent slower but cheaper.
- **✓** Fewer choices means fewer wrong picks, and less harm if one happens.
- Agents can only hold three tools.

**Why:** An agent cannot misuse a tool it does not have. Fewer, clearly labelled tools make good choices easier and bad ones less costly.

## What you can do now

- Explain what a tool is and tell read tools from write tools.
- Explain why a tool's description shapes the agent's choices.
- Argue for giving an agent only the tools it needs.

---

[Back to Agents for everyone](README.md) · [Next: Memory and context](04-memory-and-context.md)
