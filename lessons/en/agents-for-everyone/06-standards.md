# Standards, in plain words

Agents for everyone · Chapter 6 of 8 · 14 minutes

Standards are agreed message formats that let agents, tools, apps and shops built by different people work together.

## Goals

- Plug the right standard into each connection of a journey.
- Open one real message and read what its fields mean.
- Pass the check.

## The read

### Why standards

A standard is an agreed way of writing messages, a bit like the shape of a plug. If everyone builds to the same shape, any plug fits any socket. Without standards, every agent would need its own custom wiring for each app, tool and other agent it works with. With them, a company builds its side once and can connect to many others.

A message here is a small, structured piece of text that one program sends to another: who it is for, what is asked, and the details. You will see real ones in the simulation.

### Four kinds of connection

Between a person's app and an agent: AG-UI (Agent–User Interaction Protocol) carries the conversation, including the reply as it is written. A2UI (Agent-to-User Interface) lets an agent describe a screen, such as a form, for the app to draw.

Between agents: A2A (Agent2Agent Protocol) lets one agent hand a task to another, even one run by a different company, and follow it until it is done. ANP (Agent Network Protocol) is for agents that meet on the open web.

Between an agent and its tools: MCP (Model Context Protocol) lets an agent find and use tools that someone else runs, such as a helpdesk's order lookup.

Between an agent and shops or payments: UCP (Universal Commerce Protocol) gives agents a common way to check out in a shop, and AP2 (Agent Payments Protocol) lets an agent pay only with proof that you agreed.

### What a standard is, and is not

A standard is not a product, an app or a company. It is a written description of messages, and anyone can build to it. These standards are young: the first of them, MCP, appeared in late 2024, and most are newer. They still change, so when you read about one, check the date and the version.

## Try it: Plug in the standards

Connect the journey one link at a time, each with the standard made for it. Then play the journey, and open one real message.

https://agenticademy.org/en/paths/agents-for-everyone/06-standards

## Check

### Question 1 of 3

Which standard connects an agent to its tools?

- A2A
- **✓** MCP
- AG-UI

**Why:** MCP, the Model Context Protocol, is how an agent finds and calls tools that someone else runs. A2A connects agents to agents, and AG-UI connects an app to an agent.

### Question 2 of 3

What is A2A for?

- **✓** An agent asking another agent, run by someone else, to do a task.
- Drawing buttons in an app.
- Paying in a shop.

**Why:** A2A (Agent2Agent) lets one agent hand a task to another and follow it until it is done. Drawing screens is A2UI's job, and paying is AP2's.

### Question 3 of 3

What is a standard, in this sense?

- A product you buy.
- **✓** An agreed format for messages that anyone can build to.
- A law about AI.

**Why:** Like a plug shape, a standard says what messages look like, so parts made by different people fit together. It is not a product, and it is not a law.

## What you can do now

- Explain what a standard is and why agents need them.
- Name the standard for each kind of connection: apps, agents, tools, payments.
- Read the main fields of a real agent message with help.

---

[Back to Agents for everyone](README.md) · [Next: Risks, safety and saying yes](07-risks-and-approval.md)
