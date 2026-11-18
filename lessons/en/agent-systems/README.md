# Agent systems and standards

Intermediate · For developers · 8 chapters · 195 min

For developers who can already build a single agent. You connect agents to tools, to other agents, to interfaces and to payments with the open standards, orchestrate several agents in one graph, decide what goes into each context, trace runs and test them against attacks. The protocol libraries run for real in your browser; you can hold, drop and tamper with every message.

## Before you start

The Build your first agents path, or the same knowledge: LangChain.js messages and tools, LangGraph graphs and the agent loop.

## By the end you can

- Connect an agent to an MCP server using the 2026-07-28 protocol, including requests for a person's input.
- Delegate work to another agent with A2A tasks and Agent Cards.
- Stream an agent's work to an interface with AG-UI, and describe safe UI with A2UI.
- Explain how UCP and AP2 bind an agent's purchase to a person's signed consent.
- Orchestrate agents with supervisors, Send fan-out and subgraphs.
- Engineer context with search, eviction and subagents, and trace runs in LangSmith's format.
- Test an agent system against prompt injection and tampering, using the OWASP agentic risks as a map.
- Read a superstep timeline and a run tree.
- Add a call limit, a tool limit or a guard to an agent as middleware.

## Chapters

1. [MCP: tools from a server](01-mcp.md): How an agent finds and calls tools on an MCP server, and how the 2026-07-28 protocol asks a person before a risky call.
2. [A2A: agents that delegate to agents](02-a2a.md): How one agent finds another through its Agent Card and hands it a task it can follow to the end.
3. [Agents and interfaces: AG-UI and A2UI](03-agents-and-uis.md): Streaming an agent's work to an app with AG-UI, and letting an agent describe safe interface components with A2UI.
4. [Agents that pay: UCP and AP2](04-agent-commerce.md): How an agent checks out with a merchant over UCP, and how AP2 mandates bind the payment to what the person signed.
5. [Orchestration: supervisors, fan-out, subgraphs](05-orchestration.md): Patterns for several agents in one LangGraph graph: a supervisor that routes, Send for parallel work, subgraphs, and the checkpoint each superstep saves.
6. [Context engineering and retrieval](06-context-engineering.md): Choosing what each model call reads: grep or vector search, offloading large results, a first look at subagents, and memory that lasts across threads.
7. [Observability: traces and runs](07-observability.md): Recording every model call, tool call and step as a tree of runs, and reading it to find what is slow or broken.
8. [Evaluation and safety](08-evaluation-and-safety.md): Testing an agent system with cases and checks, attacking it with injection and tampering, and mapping the failures to the OWASP agentic risks.

https://agenticademy.org/en/paths/agent-systems
