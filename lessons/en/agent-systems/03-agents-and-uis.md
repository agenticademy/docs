# Agents and interfaces: AG-UI and A2UI

Agent systems and standards · Chapter 3 of 8 · 25 minutes

Streaming an agent's work to an app with AG-UI, and letting an agent describe safe interface components with A2UI.

## Goals

- Watch a run's AG-UI event stream to the end.
- Confirm the refund through a front-end tool.
- Inject a component outside the A2UI catalogue and see it refused.
- Pass the check.

## The read

### AG-UI: the agent's work as a stream

MCP connects an agent to tools and A2A to other agents. AG-UI (Agent–User Interaction protocol) connects it to the app a person uses. The app sends a RunAgentInput (thread id, run id, messages, state, and the tools the app offers) and reads back a stream of events, here as server-sent events.

A run starts with RUN\_STARTED and ends with RUN\_FINISHED or RUN\_ERROR. Text arrives as TEXT\_MESSAGE\_START, then TEXT\_MESSAGE\_CONTENT fragments, then TEXT\_MESSAGE\_END. Tool calls arrive the same way: TOOL\_CALL\_START, TOOL\_CALL\_ARGS, TOOL\_CALL\_END, and TOOL\_CALL\_RESULT for tools the agent ran. Shared state moves as STATE\_SNAPSHOT, the whole state, or STATE\_DELTA, a list of JSON Patch operations. The official client, @ag-ui/client 1.0.2, checks each event's schema and order before it applies it.

### Front-end tools

Some tools belong to the app: a confirm button, a date picker. The app lists them in RunAgentInput.tools. When the agent calls one, the run finishes with that call left pending; the app runs the tool, here by asking the customer, and starts the next run with the result as a tool message. The person's answer becomes part of the conversation, and the agent decides what to do with it.

### A2UI: interfaces as data

A2UI (protocol v0.9) lets an agent describe a piece of interface without sending code. createSurface opens a surface and names the catalogue it uses; updateComponents sends a flat list of components that refer to each other by id; updateDataModel sends the values they bind to. The app renders only components from its own catalogue, here seven types: Card, Column, Row, List, Text, Button and Divider. With strict validation, @a2ui/web\_core 0.12.0 refuses an unknown type and the surface stays as it was, so an agent, or a prompt that fooled it, cannot make the app run new code.

### What you do here

A customer asks the support chat about the double charge. Over AG-UI, watch the events stream, then confirm the refund on the card the app draws. Then switch the lab to A2UI, where the agent builds a refund card from messages. Turn on Intercept, hold the updateComponents message and add a Script component: web\_core refuses it.

## Try it: An agent builds a screen

Four steps, one action each, then two optional ones. The lab under the steps draws every message as a row, from the top down. While an A2UI lab is open, a tree under the lab shows the components the agent sent, joined by id.

https://agenticademy.org/en/paths/agent-systems/03-agents-and-uis

## Check

### Question 1 of 4

What is an AG-UI front-end tool?

- **✓** A tool the app runs, such as a confirmation dialog; its result goes back to the agent in the next run.
- A tool that draws charts on the server.
- A browser extension the agent installs.

**Why:** The agent asks for it like any other tool, but the app declares it and runs it, so a person can take part in the step. The run ends with the call pending; the result starts the next run.

### Question 2 of 4

Why does A2UI render from a catalogue instead of running the agent's markup?

- It is faster.
- **✓** The app decides which components exist, so an agent, or text it was fed, cannot make the app run new code.
- Catalogues are required by HTML.

**Why:** Describing UI as data keeps the app in control. With strict validation, a component type outside the catalogue is refused, whatever the agent sends.

### Question 3 of 4

What does an AG-UI STATE\_DELTA event carry?

- A full copy of the state.
- **✓** JSON Patch operations to apply to the shared state.
- The model's token usage.

**Why:** Deltas keep updates small. If a patch targets a path that does not exist, the client cannot apply it and keeps its previous state; a STATE\_SNAPSHOT puts it back in step.

### Question 4 of 4

The client receives TEXT\_MESSAGE\_CONTENT for a messageId that never had a TEXT\_MESSAGE\_START. What happens?

- The text is shown anyway.
- **✓** @ag-ui/client rejects the event as out of order and stops the run.
- The agent resends it.

**Why:** The client checks the order of events as well as their schema. Content for a message that never started is a protocol error, so the run stops.

## What you can do now

- Read an AG-UI event stream and say what each event does.
- Use a front-end tool to put a person inside an agent's run.
- Describe an A2UI surface: createSurface, updateComponents and updateDataModel.
- Explain how A2UI's catalogue keeps an app safe from agent-made UI.

---

[Back to Agent systems and standards](README.md) · [Next: Agents that pay: UCP and AP2](04-agent-commerce.md)
