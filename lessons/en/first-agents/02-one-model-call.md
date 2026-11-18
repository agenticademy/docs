# One model call

Build your first agents · Chapter 2 of 8 · 15 minutes

Calling a chat model with LangChain.js, reading the AIMessage it returns, and using your own key safely.

## Goals

- Run the call with the scripted model.
- Find the token counts in the reply.
- Predict what the code prints.

## The read

### One interface, many providers

Each provider package has a chat model class, such as ChatAnthropic or ChatOpenAI. They share one interface: invoke(messages) sends a list of messages and returns one AIMessage. You can also pass a plain string, which LangChain wraps in a HumanMessage. stream(messages) returns the same reply in chunks as they arrive, so you can show text while the model is still writing.

Options go to the constructor: model (the model's name at that provider), temperature (0 gives the most predictable wording; higher values vary more) and maxTokens (the longest reply you will pay for). ChatOpenAI also takes configuration.baseURL, so the same class talks to any OpenAI-compatible service: OpenRouter, Hugging Face's router or NVIDIA's API catalog.

### What comes back

The AIMessage has content (the text, for a plain reply), tool\_calls (empty until chapter 5), usage\_metadata and response\_metadata. usage\_metadata counts tokens, the pieces of text a model reads and writes: input\_tokens for what you sent, output\_tokens for the reply. Providers bill by tokens, and the input grows with every message you send, so this is the number to watch. response\_metadata holds what the provider adds, such as the model name and why it stopped.

### Keys

An API key is a password for a paid account. Keep it out of code and out of repositories: read it from an environment variable on your machine or server. In this page, a key you connect stays in this tab and goes from your browser straight to the provider. That is fine for learning, but a public app should call models from a server, so its key is never sent to visitors. Free tiers exist and are rate limited.

## Try it: A request and its reply

Run the call and open the AIMessage that comes back. Connect your own model if you want to see a live reply.

https://agenticademy.org/en/paths/first-agents/02-one-model-call

## Check

The model is a chat model that replies "Hello." and reports its usage. What does this print?

Code:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- It throws: invoke needs an array of messages.

**Why:** invoke also accepts a plain string, which LangChain wraps in a HumanMessage. For a text reply, content is a string, and usage\_metadata counts the output tokens.

## What you can do now

- Call a chat model with invoke() and stream().
- Read content and token usage from an AIMessage.
- Point ChatOpenAI at an OpenAI-compatible service with baseURL.
- Explain where an API key should and should not live.

---

[Back to Build your first agents](README.md) · [Next: Messages and state](03-messages-and-state.md)
