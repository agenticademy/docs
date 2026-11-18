# MCP: tools from a server

Agent systems and standards · Chapter 1 of 8 · 25 minutes

How an agent finds and calls tools on an MCP server, and how the 2026-07-28 protocol asks a person before a risky call.

## Goals

- Run the scenario until the refund is issued.
- Answer the helpdesk's confirmation yourself.
- Tamper with a message and see the real SDK reject it.
- Pass the check.

## The read

### Hosts, clients and servers

The Model Context Protocol (MCP) is an open standard for connecting AI applications to tools and data. The application, called the host, runs one MCP client per server it uses. A server offers tools (actions the model can call), resources (data the application can read) and prompts (templates a person can choose). Messages are JSON-RPC 2.0, over HTTP or over a local process's standard input and output (stdio).

In the agent from the previous path, tools were functions in your code. With MCP they live in a server that someone else can write and run, and any MCP host can use them.

### What 2026-07-28 changed

This chapter uses protocol version 2026-07-28 with the official TypeScript SDK v2. You will see three changes on the wire.

Discovery: the client calls server/discover to learn the server's versions and capabilities. Servers on the 2025 versions answer initialize instead, and the SDK falls back to it.

A version on every request: each request carries its protocol version in \_meta, so the server keeps no session, and any instance of it can answer any request.

Input required: when a tool needs a person's answer, the server ends the request with input\_required, the question, and a signed requestState. The client asks the person, then calls the tool again with the answer and the same requestState. The server checks the signature, so the retry must match what the person saw.

### Tools are a contract

The model sees only a tool's name, description and JSON Schema. Annotations such as readOnlyHint and destructiveHint are hints from the server: a client trusts them only as far as it trusts that server. The refund in this lab is safe because the server itself asks for a confirmation and checks it, not because the model behaves.

### What you do here

Halden's support agent, a LangChain agent with the SDK's client, and Halden's helpdesk, the SDK's server, both run in this page. Run the scenario and answer the refund question yourself. Then turn on Intercept, hold a message and tamper with it: the real SDK code rejects it.

## Try it: The wire: an agent and an MCP helpdesk

Four steps, one action each, and an optional fifth. The diagram under the steps has one column per party and one row per message, in the order it is sent, from the top down.

https://agenticademy.org/en/paths/agent-systems/01-mcp

## Check

### Question 1 of 4

In MCP, who runs the client?

- The server.
- **✓** The host application, for example the support agent.
- The model provider.

**Why:** The host, such as an agent or an IDE, runs one client per server it connects to. The model never talks to the server directly; the host does, when the model asks for a tool.

### Question 2 of 4

The helpdesk needs the support lead's OK before a refund. What does a 2026-07-28 server send?

- A new request from the server to the client, asking for input.
- **✓** A result with input\_required, the question and a signed requestState, which ends the request.
- Error -32602.

**Why:** The server ends the request instead of waiting on an open connection. The client asks the person and retries with the answer and the same requestState, so any server instance can finish the job.

### Question 3 of 4

On the retry, you raise the refund amount. Why does the server reject it?

- The model notices the change.
- **✓** The arguments no longer match the signed requestState, which was minted for what the person approved.
- MCP does not allow amounts in arguments.

**Why:** requestState is signed by the server (HMAC here). The server reads back the approved order and amount and refuses any call that differs.

### Question 4 of 4

A server marks a tool readOnlyHint: true. What should the client do?

- Run it without ever asking anyone, since it cannot change anything.
- **✓** Treat it as a hint, trusted only as far as the client trusts that server.
- Refuse to run it.

**Why:** Annotations describe what the server claims. A malicious or buggy server can mislabel a tool, so a client's own policy decides what needs approval.

## What you can do now

- Describe an MCP host, client and server, and the three things a server offers.
- Follow the 2026-07-28 flow: server/discover, tools/list, tools/call, input\_required and the retry.
- Explain why a signed requestState keeps a confirmation bound to what the person saw.
- Treat tool annotations as hints, not guarantees.

---

[Back to Agent systems and standards](README.md) · [Next: A2A: agents that delegate to agents](02-a2a.md)
