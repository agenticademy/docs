# Open and closed models

Understanding models · Chapter 6 of 8 · 15 minutes

Some models can only be used through their maker's service, some share their weights under licences from strict to permissive, and a few are open source, with their data and code.

## Goals

- Pick a goal, such as running offline or shipping a commercial app.
- Check the models level by level and see which qualify for that goal.
- Open one licence and read its key terms.
- Pass the check.

## The read

### Closed: a service you call

In chapter 4, open models published their weights, the numbers a model learned, and closed ones did not. A closed model runs on its maker's computers. You reach it through the maker's app or its API, a service your program sends text to, and pay per token under the maker's terms of service. Claude, GPT, Gemini and Grok work this way.

### Open weights, under a licence

An open-weights model lets you download its weights. You can run it on your own computer with no internet connection, and fine-tune it: train it further on your own examples. A licence comes with the weights, the legal text that says what you may do with them.

A permissive licence, such as Apache-2.0 or MIT, allows almost any use, commercial too, as long as you keep its notice. A restrictive licence adds conditions, such as a separate deal above a number of users or an amount of revenue, a name to show, or uses that are not allowed. Gemma 4, gpt-oss and DeepSeek-V4.1-Flash have permissive licences; Llama 4 and Kimi K3 have restrictive ones.

### Open source

Open weights are not open source. The Open Source Initiative (OSI), which keeps the definition of open-source software, wrote the Open Source AI Definition 1.0. It asks for three things under open terms: the weights, the code that trains and runs the model, and data information, enough about the training data for a skilled person to build a similar model. Few models meet it. Olmo 3, which you followed in chapter 4, publishes all three.

### Getting and running them

Open models are shared on hubs such as Hugging Face, a website that holds each model's files, model card and licence. Some are gated: you accept the licence, and sometimes wait for the maker's approval, before the download starts. On your computer, apps run them from files made for them: llama.cpp and Ollama read GGUF files, MLX runs models on Macs with Apple silicon, and a browser with WebGPU can run small ones, as in chapter 3. Whether one fits is chapter 5's question.

### In the visual

You pick a goal and check real models level by level, from closed to open source. Then you open one licence. The facts come from the models' pages and licences, in a snapshot dated 2026-11-03. It is a summary to learn from, not legal advice.

## Try it: Licence matrix

Pick a goal, check the models level by level from closed to open source, then open one licence and read its key terms.

https://agenticademy.org/en/paths/understanding-models/06-open-and-closed

## Check

### Question 1 of 3

You need a model on a laptop with no internet connection. Which kind can do that?

- A closed model, used through its API from the laptop.
- **✓** An open-weights or open-source model, downloaded to the laptop first.
- Any model at all, as long as you have an API key for it.
- None of them: every model needs the internet to answer.

**Why:** Running offline needs the weights on the laptop, and only open-weights and open-source models let you download them. A closed model runs on its maker's servers, so its API needs a connection, with a key or without. A downloaded open model needs none, if the laptop has the memory for it.

### Question 2 of 3

A model's weights are published under Apache-2.0, but nothing is said about its training data. Which level is it?

- Closed, since nothing is said about its training data.
- Open weights, restrictive licence.
- Open source, by OSI's definition.
- **✓** Open weights, permissive licence.

**Why:** Closed means the weights stay with the maker, and these are published, whatever is said about the data. Apache-2.0 allows commercial use with no user or revenue limits, so the licence is permissive, not restrictive. It is not open source: OSI's definition also asks for data information and the training code, and a permissive licence alone does not give them.

### Question 3 of 3

Llama 4's licence says that above 700 million monthly active users you need a separate licence from Meta. A start-up with 20,000 users builds a paid app on Llama 4. What does that condition mean for it?

- It may not use Llama 4 in a paid app until Meta gives it a separate licence.
- It must pay Meta per token to use Llama 4, as it would with a closed model.
- **✓** The user limit does not apply to it, but the licence's other terms already do.
- Nothing in the licence applies to it until it has 700 million monthly users.

**Why:** The limit starts above 700 million monthly active users, so a small start-up may build a paid app without a separate licence. The other terms apply from the first user: Llama 4's licence asks you to show “Built with Llama” and to follow Meta's use policy. Open weights are not paid per token: you run them yourself.

## What you can do now

- Tell closed models, open weights under restrictive or permissive licences, and open source apart.
- Say which kinds of model suit a goal: a commercial app, offline use, fine-tuning or known training data.
- Read a licence's key terms, such as user and revenue limits, before you build on a model.
- Name where open models are shared and the apps that run them on your own computer.

---

[Back to Understanding models](README.md) · [Next: Comparing models](07-comparing-models.md)
