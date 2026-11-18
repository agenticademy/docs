# Messages and state

Build your first agents · Chapter 3 of 8 · 18 minutes

The four message types, why the whole conversation is sent every time, and how LangGraph merges state with reducers.

## Goals

- Change the system message and compare the replies.
- Append one message and replace another by its id.
- Predict what the reducer returns.

## The read

### Four message types

A conversation is a list of messages, and each message has a type. SystemMessage holds your instructions to the model. HumanMessage is what the user wrote. AIMessage is the model's reply; it may carry tool\_calls, requests to run tools. ToolMessage carries a tool's result back, linked to its request by tool\_call\_id. All four come from @langchain/core/messages.

### The model remembers nothing

A chat model keeps no memory between calls. Each call sends the whole list, and the reply depends only on that list. To continue a conversation, you append the reply and the next question, and send everything again. That is also why the input token count grows with every turn.

### State and reducers

In LangGraph, the data a graph carries from step to step is its state. A node does not overwrite the state: it returns an update, and a reducer decides how the update merges into what is there. Without a reducer, the new value replaces the old one.

For messages, LangGraph provides MessagesAnnotation, a state with one key, messages, merged by messagesStateReducer. Every message gets an id; the reducer assigns one if it has none. A message with a new id is appended. A message with an id already in the list replaces that message in place. A RemoveMessage with an id deletes that message. This is how a graph edits a conversation, for example to trim old turns, without rebuilding the list.

## Try it: The message stack

Call the model, change the system message and call again, then send state updates and watch the reducer append, replace and remove messages.

https://agenticademy.org/en/paths/first-agents/03-messages-and-state

## Check

What does messagesStateReducer return?

Code:

```
messagesStateReducer(
  [new HumanMessage({ id: "a", content: "Hi" }), new AIMessage({ id: "b", content: "Hello" })],
  [new AIMessage({ id: "b", content: "Hello!" }), new HumanMessage({ id: "c", content: "Thanks" })],
);
```

- Four messages: a, b, b, c.
- **✓** Three messages: a, b (now "Hello!"), c.
- Two messages: b and c.
- It throws because b appears twice.

**Why:** The update with id b replaces the existing b in place, and c is new, so it is appended.

## What you can do now

- Use the four message types correctly.
- Explain why the conversation is sent with every call.
- Predict how LangGraph's messages reducer merges an update.

---

[Back to Build your first agents](README.md) · [Next: Your first graph](04-your-first-graph.md)
