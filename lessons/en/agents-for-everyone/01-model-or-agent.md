# A model answers. An agent acts.

Agents for everyone · Chapter 1 of 8 · 10 minutes

The difference between an AI model, which gives you text, and an agent, which takes steps to reach a goal.

## Goals

- Send the same request to a model and to an agent, and let both finish.
- Find the step where the agent changed something outside the conversation.
- Pass the check.

## The read

### A model writes

An AI model, like the one behind a chatbot, is a program trained on a very large amount of text. Given some words, it predicts the words that should come next. That is enough to answer a question, summarise a letter or draft an email.

Models are made by companies and research groups. Some models are open: anyone can download and run them. Many are shared on Hugging Face, a website for sharing models, and some, such as NVIDIA's Nemotron family, can be tried free, with limits.

On its own, a model can only give you text back. It cannot look at your calendar or book anything. Whatever it suggests, you still have to do yourself.

### An agent does

An agent is a program built around a model. You give it a goal, and it can use tools: other programs that look things up or change things, such as a calendar, a map or a booking site.

The agent works in steps. The model decides what to do next, the agent uses the tool, and the model reads what came back. This repeats until the goal is reached, or until the agent needs you. "Agentic AI" is the general name for systems that work this way.

### Same request, two results

Ask a model to book a table for six on Saturday and it tells you how to book one. Ask an agent and it can check your calendar, find a restaurant, ask you to confirm, and make the booking. The simulation below plays both, so you can compare them.

### Why the difference matters

Text is easy to ignore. Actions are not: a booking, a payment or a sent email has effects in the world. That is why a good agent asks before it does something that matters, and why this path spends a whole chapter on safety.

### Four words for this path

Model: the program that reads and writes text. Tool: something an agent can use to look things up or to act. Goal: what you asked the agent for. Step: one decision by the model, and one action by the agent.

## Try it: One request, two results

Give the same request to a model, then to an agent. Answer the agent when it asks, then find the step that changed something in the world.

https://agenticademy.org/en/paths/agents-for-everyone/01-model-or-agent

## Check

### Question 1 of 4

You ask a chatbot for a recipe and it writes one out. Is that an agent at work?

- Yes, because it used AI.
- **✓** No. It gave text back, and nothing was done for you.
- Yes, because it understood what you meant.

**Why:** Writing a recipe is what a model does on its own. An agent would use tools to act, for example checking what is in your online grocery order and adding the missing items.

### Question 2 of 4

What does an agent have that a model on its own does not?

- A bigger memory of the internet.
- **✓** Tools it can use, and a loop of steps that goes on until the goal is reached.
- A voice and a face.

**Why:** An agent is a model plus tools plus a loop: decide, act, read the result, decide again. The model inside can be exactly the same one a chatbot uses.

### Question 3 of 4

In the simulation, why did the agent ask you before booking?

- **✓** A booking has real effects, so a person should confirm it first.
- It did not know how to book.
- Agents ask before every single step.

**Why:** Looking things up is harmless; booking is not. Good agents ask before actions that are hard to undo or that commit you to something, and leave the rest to themselves.

### Question 4 of 4

Which of these is a tool, in the sense used here?

- The text the model was trained on.
- Your request.
- **✓** A calendar the agent can read.

**Why:** A tool is something the agent can use during its steps to look things up or to act. Your request is the goal, and the training text is what the model learned from.

## What you can do now

- Tell a model from an agent by asking one question: can it act, or only answer?
- Name the parts of an agent: a model, tools, a goal and a loop of steps.
- Explain why an agent should ask you before an action that matters.

---

[Back to Agents for everyone](README.md) · [Next: The loop: look, decide, act](02-the-agent-loop.md)
