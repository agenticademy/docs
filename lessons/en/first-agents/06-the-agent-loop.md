# The agent loop: a minimal ReAct agent

Build your first agents · Chapter 6 of 8 · 20 minutes

A model node, a tool node and one conditional edge: the whole agent loop as a LangGraph graph.

## Goals

- Step the agent one superstep at a time until it answers.
- Count how many times the model ran.
- Put the run in order.

## The read

### Reason, act, observe, repeat

ReAct (reason and act) is a pattern for agents: the model reads the conversation and either asks for tools or answers. When it asks, your code runs the tools and adds their results to the conversation, and the model reads again. The loop ends when the model answers without asking for a tool.

### The loop as a graph

In LangGraph the loop is two nodes over MessagesAnnotation, the state that holds the list of messages. The agent node calls the model, with the tools bound, on state.messages and returns its reply. The tools node is ToolNode from @langchain/langgraph/prebuilt: it runs every tool call in the last AIMessage and adds one ToolMessage per call. Invalid arguments become error ToolMessages, as in chapter 5.

toolsCondition is the router after the agent node: it returns "tools" when the last message has tool\_calls and END when it has none. One edge leads from tools back to agent. That is the whole agent, about 25 lines.

LangGraph runs a graph in supersteps: in each one, the nodes that are due run once. In this graph only one node is due at a time, so each superstep is one node run, and graph.stream(input, { streamMode: "updates" }) yields one update per superstep. You can watch the loop turn.

### Ready-made, and capped

createReactAgent in @langchain/langgraph/prebuilt builds the same graph; it is deprecated in favour of createAgent in the langchain package. Build it by hand once and you will know what those helpers do.

A loop can run forever if the model keeps asking for tools. recursionLimit in the run's config caps the supersteps (25 by default); when it is reached, LangGraph throws GraphRecursionError. Try a limit of 4 below: this run needs 5 supersteps.

## Try it: The loop, one step at a time

Three steps, one action each, and an optional fourth: send ticket T-48213, step the loop until the agent answers, count the model calls, then cap the loop with recursionLimit.

https://agenticademy.org/en/paths/first-agents/06-the-agent-loop

## Check

Put the run of ticket T-48213 in order.

1. The agent node sends the task to the model.
2. The model asks for get\_ticket.
3. ToolNode runs get\_ticket and adds a ToolMessage.
4. The model reads it and asks for list\_charges.
5. ToolNode runs list\_charges and adds a ToolMessage.
6. The model answers without tool calls.
7. toolsCondition routes to END.

**Why:** Each model turn either asks for tools, which loops through ToolNode, or answers, which ends the run. The model ran three times.

## What you can do now

- Build a ReAct agent as a graph with ToolNode and toolsCondition.
- Trace a run message by message with streamMode "updates".
- Explain why every agent loop needs a recursion limit.

---

[Back to Build your first agents](README.md) · [Next: Callbacks and human approval](07-callbacks-and-approval.md)
