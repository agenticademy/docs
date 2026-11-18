# Choosing a model for an agent

Understanding models · Chapter 8 of 8 · 18 minutes

Choosing a model means removing the ones that fail a hard limit, then weighing capability, speed, price per task, context and reliability with tools for one agent's job.

## Goals

- Pick an agent to choose a model for.
- Apply its hard limits and see which models drop out.
- Weigh what is left, rank it and price a month of work.
- Pass the check.

## The read

### A fit, not a best

There is no best model, only the best fit for one agent's job. Choosing takes two passes. First come the hard limits: needs the agent cannot do without. A model that fails one is out, however strong or cheap it is. Then you weigh what is left: you decide how much each quality matters for this job, and rank the models by it.

### Hard limits

Modality: the kinds of input and output a model handles, such as text, images or sound. Context window: how many tokens it reads at once, as in chapter 2. Data policy: whether the provider may train its models on what you send, and retention, how long it keeps it. Region: where your data is processed; some rules, such as the EU's, care where. Licence and access: what you may do with a model, and who may use it at all. Open weights can also run on your own machine, even with no internet.

### What you weigh

Capability: how well it does the job. Benchmarks, as in chapter 7, help when their tasks are close to yours, but your own eval says more: a few dozen real tasks from the job, scored by you. Latency: how long you wait for an answer; more thinking usually means better answers and longer waits.

Price per task: what one finished task costs. An agent calls its model several times per task, and each call is a turn, billed for its tokens in and out. A cheap price per token can still mean an expensive task. Tool-call reliability: how often the model calls tools correctly, with the right inputs. Rate limits: how many requests and tokens a provider accepts per minute.

### In the visual

You pick one of four agents. Its hard limits remove models, your weights rank the rest, and a calculator turns real list prices into a monthly bill. A worked answer then shows what its choice gives up. The agents and their task sizes are made up. The models, prices, context windows, inputs, licences and scores are real, from a snapshot dated 2026-11-03. The snapshot has no regions and only some data terms, so some limits say Check; it has no speeds or rate limits, so you test those yourself.

## Try it: Choose a model for this agent

Pick an agent, apply its hard limits one at a time, weigh what is left and price a month of work, then compare with a worked answer.

https://agenticademy.org/en/paths/understanding-models/08-choosing-for-an-agent

## Check

### Question 1 of 3

A support bot for a European bank must keep customer data in the EU. Two models can do the job; the cheaper one runs only on servers outside the EU. What comes first?

- Pick the cheaper one: price matters most.
- **✓** Drop the one that cannot keep the data in the EU, then compare the rest.
- Pick the one with the higher leaderboard score.
- Use both and let them vote.

**Why:** Keeping data in the EU is a hard limit: a model that fails it is out, however cheap or strong it is. Price and scores only rank the models that are allowed. Letting both vote would still send the data outside the EU.

### Question 2 of 3

Model B costs twice as much per token as model A. In your eval, B finishes a task in 2 turns, and A needs 5 turns of the same size. Which costs less per task?

- A, because its price per token is half of B's.
- **✓** B, because 2 turns at twice the price cost less than 5 turns.
- Neither: with the same tokens per turn, the price per token decides.
- You cannot tell without a leaderboard score.

**Why:** Price per task is the price per token times the tokens one task uses. If one turn costs 1 on A, it costs 2 on B, so A's task costs 5 and B's costs 4. The price per token alone misses the number of turns, and a leaderboard score does not count your turns at all; your own eval does.

### Question 3 of 3

The top model on your shortlist has the best independent score, but the snapshot says nothing about its speed or how reliably it calls tools. What do you do before you commit?

- Nothing: the best score means it is also the fastest and most reliable.
- **✓** Run your own eval: real tasks from the job, timing the replies and checking every tool call.
- Pick the cheapest model instead.
- Wait for next month's leaderboard.

**Why:** A score measures its benchmark's tasks at one setting; it does not time replies on your job or check your tools. Your own eval, a few dozen real tasks you score yourself, measures exactly that. Picking on price alone, or waiting for a newer leaderboard, leaves the same gap.

## What you can do now

- Separate the hard limits that remove a model from the qualities you weigh.
- Work out a price per task and per month from tokens, turns and list prices.
- Rank a shortlist by what matters for one agent, and say what the choice gives up.
- Name what a model's page cannot tell you, and test it in your own eval.

---

[Back to Understanding models](README.md)
