# Build your first agents

Beginner · For developers · 8 chapters · 152 min

For developers starting with agents. You build one small support agent step by step: a model call, messages and state, a graph, tools, the agent loop, callbacks with human approval, and guardrails. Every chapter runs the real libraries in your browser with a scripted model, so you need no key; you can connect your own model when you want.

## Before you start

Some JavaScript: functions, objects, async/await and installing packages with npm.

## By the end you can

- Choose the LangChain.js and LangGraph.js packages for an agent, and know what each one does.
- Call a chat model and work with messages and graph state.
- Build a StateGraph with nodes, edges and conditional routing.
- Define tools with zod schemas and run a minimal ReAct agent.
- Pause a run for human approval with checkpoints and interruptBefore, and log it with callbacks.
- Add guardrails: input and output checks, tool allow-lists and step caps.

## Chapters

1. [The libraries, and what runs here](01-libraries-and-setup.md): Which LangChain.js and LangGraph.js packages an agent needs, what each one does, and how they run in a browser.
2. [One model call](02-one-model-call.md): Calling a chat model with LangChain.js, reading the AIMessage it returns, and using your own key safely.
3. [Messages and state](03-messages-and-state.md): The four message types, why the whole conversation is sent every time, and how LangGraph merges state with reducers.
4. [Your first graph](04-your-first-graph.md): Building a StateGraph from nodes and edges, routing with a conditional edge, and running it step by step.
5. [Tools and tool calls](05-tools-and-tool-calls.md): Defining tools with zod, binding them to a model, and handling the tool calls the model makes.
6. [The agent loop: a minimal ReAct agent](06-the-agent-loop.md): A model node, a tool node and one conditional edge: the whole agent loop as a LangGraph graph.
7. [Callbacks and human approval](07-callbacks-and-approval.md): Watching a run with callbacks, and pausing it before a risky tool so a person can approve or reject it.
8. [Guardrails](08-guardrails.md): Checks in code around the model: input and output checks, tool allow-lists and step caps, in layers.

https://agenticademy.org/en/paths/first-agents
