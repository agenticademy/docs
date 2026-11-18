# Kinds of models

Understanding models · Chapter 1 of 8 · 14 minutes

Chat, reasoning, multimodal, embedding, reranking, speech, image, guard and small models do different jobs, and one agent often uses several.

## Goals

- Fit the first slot of the voice agent: turn the caller's voice into text.
- Fit the two slots that find the notes that answer the question.
- Fit the last three slots: write the reply, check it and say it.
- Pass the check.

## The read

### Different models, different jobs

A model is a program that learned patterns from examples. Models learn different jobs. Each takes in some kind of input, such as text, sound or images, and gives out some kind of output. An agent often chains several models, each doing the job it does best.

### Models that write

A chat model, also called an instruct model, follows instructions and writes a reply. A reasoning model works through a problem step by step before it answers: better on hard problems, but slower. Many new models do both, with a setting for how much they think. A multimodal model also reads images, sound or video.

### Models that find and sort

An embedding model turns a text into a list of numbers, so texts with similar meanings get similar numbers. A search can then compare the numbers and find notes close in meaning to a question, quickly, even among thousands. A reranker reads the question together with each note it is given and puts the best first. That is more careful but slower, so it only sorts the few notes the search found.

### Models that listen, speak, draw and check

Speech to text writes down what someone says. Text to speech reads text out loud. An image model makes a picture from a description. A guard model, also called a safety classifier, labels text or images as safe or unsafe, so an agent can stop a bad reply before anyone sees or hears it.

### Small, closed and open

A model's size is counted in parameters: the internal values it adjusted in training, also called its weights. Small models have a few billion parameters or fewer, so they can run on a phone or a laptop. A closed model's weights are not published, and most closed models are used through an online service. An open-weights model can be downloaded and run on your own computer.

In the visual you fit out a voice agent for a library's phone line, one slot at a time. The library is made up; every example model you meet is real, as of 2026-11-03.

## Try it: Fit out a voice agent

Fit one slot at a time, from the top: read what the slot takes in and must give out, then pick the model card that matches.

https://agenticademy.org/en/paths/understanding-models/01-kinds-of-models

## Check

### Question 1 of 3

A support agent must find the three help articles that best match a customer's question, out of 5,000. Which pair of models is made for that job?

- An image model and a speech-to-text model.
- **✓** An embedding model to find articles close in meaning, then a reranker to put the best first.
- A guard model and a text-to-speech model.
- A reranker on its own, reading all 5,000 articles with the question.

**Why:** Embeddings let a search compare numbers, so it finds similar articles quickly even among thousands. A reranker then reads the question with each of the few found and puts the best first. A reranker on its own would read all 5,000 pairs for every question, which is slow. Image, speech, guard and text-to-speech models do other jobs: pictures, sound and safety labels.

### Question 2 of 3

A field app must answer questions on a phone with no internet connection. Which kind of model can do that?

- A closed chat model, used through its maker's service.
- **✓** A small open-weights model, downloaded onto the phone.
- A reasoning model on a server.
- An embedding model on its own.

**Why:** With no connection, the model must run on the phone itself. A small open-weights model can: you download it once, and it runs without the internet. A closed chat model used through its maker's service, and a reasoning model on a server, both need a connection to be reached. An embedding model gives out numbers, not answers.

### Question 3 of 3

A caller asks a voice agent an easy question and waits on the line. Why does a chat model fit the reply slot better than a reasoning model?

- A reasoning model cannot read text.
- **✓** A reasoning model works through steps before it answers, so the caller waits longer for an easy answer.
- A chat model is always right.
- A reasoning model gives out sound, not text.

**Why:** Both take in text and give out text. A reasoning model spends time thinking before it answers, which helps on hard problems but leaves the caller in silence on an easy one. No model is always right, and turning text into sound is the job of text to speech.

## What you can do now

- Name ten kinds of models and say what each takes in and gives out.
- Pick a kind of model for each step of a simple agent, and say why another kind would not fit.
- Give a closed and an open example of each kind, from a dated snapshot.

---

[Back to Understanding models](README.md) · [Next: Tokens](02-tokens.md)
