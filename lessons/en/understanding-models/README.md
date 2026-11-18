# Understanding models

Beginner · No code · 8 chapters · 120 min

For anyone who wants to know what is inside the models behind AI tools. You see how text becomes tokens, how a model picks the next one, how models are made, how big they are and what their licences allow, then you choose a model for an agent. Examples pair closed models with open ones, and every figure comes from a dated snapshot.

## Before you start

Nothing. The first two chapters of AI principles help.

## By the end you can

- Name the kinds of models an agent uses, and what each one does.
- Explain tokens, how a model picks the next one, and what temperature changes.
- Follow how a model is made, from data and pre-training to a quantised file.
- Tell closed models, open weights and open source apart, and read a licence's key terms.
- Read a leaderboard with care, and choose a model for an agent by capability, cost and policy.

## Chapters

1. [Kinds of models](01-kinds-of-models.md): Chat, reasoning, multimodal, embedding, reranking, speech, image, guard and small models do different jobs, and one agent often uses several.
2. [Tokens](02-tokens.md): Models read and write text as tokens, each family of models splits text its own way, and both prices and context limits are counted in tokens.
3. [Choosing the next token](03-next-token.md): A model scores every possible next token, turns the scores into chances and picks one at random; temperature, top-k and top-p change the pick, and base, instruct and reasoning models are trained to write differently.
4. [Where models come from](04-where-models-come-from.md): A model is made in stages, from data and pre-training to fine-tuning, feedback, distillation and quantisation, and for open models many stages leave a file you can inspect.
5. [Size and memory](05-size-and-memory.md): The memory a model needs comes from its parameters and the bits that store each one, plus room for its context; a mixture of experts uses a few of its parameters for each token but keeps them all in memory.
6. [Open and closed models](06-open-and-closed.md): Some models can only be used through their maker's service, some share their weights under licences from strict to permissive, and a few are open source, with their data and code.
7. [Comparing models](07-comparing-models.md): Benchmarks and leaderboards help you compare models, but margins of error, effort settings, cost, leaked tests, maxed-out tests, the test program and who ran it all change what a score means.
8. [Choosing a model for an agent](08-choosing-for-an-agent.md): Choosing a model means removing the ones that fail a hard limit, then weighing capability, speed, price per task, context and reliability with tools for one agent's job.

https://agenticademy.org/en/paths/understanding-models
