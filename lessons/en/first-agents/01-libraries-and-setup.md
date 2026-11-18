# The libraries, and what runs here

Build your first agents · Chapter 1 of 8 · 15 minutes

Which LangChain.js and LangGraph.js packages an agent needs, what each one does, and how they run in a browser.

## Goals

- Open the six packages in the map and read what each one gives you.
- Run the hello-world in this page and read the reply.
- Pass the check.

## The read

### The packages

LangChain.js and LangGraph.js are open-source JavaScript libraries for building with language models. They come as separate packages, so you install only what you use.

@langchain/core holds the shared building blocks: messages, the chat model interface, tools and callbacks. @langchain/langgraph is the runtime for agents as graphs: state, nodes, edges, checkpoints and pauses for approval. A provider package such as @langchain/anthropic or @langchain/openai gives you the chat model class for one provider's API; the same ChatOpenAI class also talks to OpenAI-compatible services such as OpenRouter, Hugging Face and NVIDIA. zod describes and validates the input of your tools; LangChain turns a zod schema into the JSON Schema the model reads. langsmith is optional and records traces of each run.

### Why a graph library

You can write an agent loop by hand: call the model, run the tools it asks for, repeat. Chapter 6 builds that loop as a graph of two nodes. LangGraph adds what a hand-written loop lacks: state saved after every step, so a run can pause for a person and resume later; branches that run in parallel; and a structure you can inspect and draw.

The langchain package builds on LangGraph and offers createAgent, a ready-made agent with middleware. This path builds the graph by hand first, so that later you know what such helpers do for you.

### Install

In your own project: npm install @langchain/core @langchain/langgraph @langchain/anthropic zod. This path uses @langchain/core 1.2.18, @langchain/langgraph 1.5.1 and zod 4.6.7. Pin exact versions in real projects: these libraries move quickly.

### What is different in a browser

Everything in this path runs in your browser, so it imports LangGraph from @langchain/langgraph/web. The main entry point passes configuration down to nested calls with AsyncLocalStorage, which browsers do not have. With /web you pass config explicitly to nested calls and pause runs with breakpoints (interruptBefore) instead of interrupt(). The rest of the API is the same.

The model is scripted by default, so you need no key and every run gives the same result. In this chapter it is LangChain's own FakeListChatModel, which returns the replies you give it. From chapter 2 you can connect a real model with your own key.

## Try it: The packages

Open each package in the map to see what it gives you, the version used here and how to import it. Then run the hello-world.

https://agenticademy.org/en/paths/first-agents/01-libraries-and-setup

## Check

### Question 1 of 4

Which package defines HumanMessage, AIMessage and tool()?

- @langchain/langgraph
- **✓** @langchain/core
- @langchain/anthropic
- zod

**Why:** @langchain/core holds the shared building blocks. LangGraph and the provider packages import them from there.

### Question 2 of 4

You run a LangGraph graph in a browser. Where do you import StateGraph from?

- @langchain/langgraph
- langchain
- **✓** @langchain/langgraph/web

**Why:** The /web entry point does not depend on AsyncLocalStorage, which browsers lack. You then pass config explicitly to nested calls.

### Question 3 of 4

What does zod do in a LangChain agent?

- **✓** It describes and validates tool inputs; LangChain sends the schema to the model as JSON Schema.
- It calls the model.
- It saves checkpoints between steps.

**Why:** The schema serves twice: the model reads it to know what arguments to send, and your code uses it to reject bad arguments.

### Question 4 of 4

Why does the hello-world use FakeListChatModel?

- It reasons better than real models.
- Real models cannot be called from a browser.
- **✓** It returns fixed replies, so the code runs with no key and no network, the same way every time.

**Why:** A fake model makes code testable and free to run. Real models can be called from a browser with a key, which chapter 2 shows.

## What you can do now

- Pick the packages for a LangGraph.js agent and say what each one does.
- Install them with pinned versions.
- Explain what changes when LangGraph runs in a browser.
- Run LangChain code with a fake model, no key needed.

---

[Back to Build your first agents](README.md) · [Next: One model call](02-one-model-call.md)
