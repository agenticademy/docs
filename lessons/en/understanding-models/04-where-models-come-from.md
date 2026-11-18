# Where models come from

Understanding models · Chapter 4 of 8 · 15 minutes

A model is made in stages, from data and pre-training to fine-tuning, feedback, distillation and quantisation, and for open models many stages leave a file you can inspect.

## Goals

- Follow Olmo 3, a fully open model, from its data to its release.
- Follow DeepSeek-R1 to a small model distilled from it.
- Follow Gemma 4 from its base model to a quantised file.
- Pass the check.

## The read

### Data, tokens and pre-training

Every model starts with data: large collections of text, code and often images, filtered, with copies removed. A tokenizer, as in chapter 2, splits it into tokens.

Pre-training shows the model trillions of tokens and adjusts its weights, also called parameters: the numbers it learns. It gets better at predicting the next token, the task from chapter 3. Labs save checkpoints, copies of the weights along the way. Some, like Olmo 3, end with mid-training, a shorter phase on a smaller, chosen mix, and a phase on long texts. The result is a base model, which continues text but does not follow requests.

### Post-training

Post-training makes a base model follow requests. Supervised fine-tuning (SFT) trains it on examples of requests with good answers.

Reinforcement learning (RL) then trains it with rewards: answers that score well become more likely. In RLHF, RL from human feedback, people compare answers and the scores come from their choices. In RLAIF another model gives the feedback, sometimes guided by a written list of principles called a constitution. DPO, direct preference optimisation, learns from pairs of a better and a worse answer in one simpler step.

Reasoning models add RL with verifiable rewards (RLVR): an answer is rewarded only when a program confirms it, such as a maths result or code that passes its tests.

### Smaller and lighter

Distillation trains a small model, the student, on answers written by a large one, the teacher. Quantisation stores each weight with fewer bits, the 0s and 1s of computer memory, for example 4 instead of 16, so the file shrinks and fits smaller machines, with a small loss of quality. Quantisation-aware training (QAT) trains a model to lose less when it is quantised. GGUF is a file format read by apps that run models on your own computer.

### Tested and described

Before release, labs evaluate a model on benchmarks, fixed sets of test questions, and red-team it: testers try to make it do harm. A model card describes the model, its training and its results; a system card also covers the safeguards around it.

Open models publish their weights; fully open ones also publish their data and code. Closed models such as Claude, GPT, Gemini and Grok are trained too, but their makers publish reports, not weights, so their trees cannot be followed file by file. The visual follows three open ones.

## Try it: A model's family tree

Follow three real trees from the top, one stage at a time. Each stage shows what it used and made, and where that is published.

https://agenticademy.org/en/paths/understanding-models/04-where-models-come-from

## Check

Put the stages of making a model in order, from first to last.

1. Collect and clean the data, and split it into tokens.
2. Pre-train on predicting the next token: the result is a base model.
3. Fine-tune it on examples of requests with good answers.
4. Train it further with rewards: from feedback, and from answers a program can check.
5. Distil a smaller student model from its answers.
6. Quantise the student's weights to fewer bits.
7. Evaluate and red-team the result, then publish its model card.

**Why:** Pre-training reads tokens, so the data and the tokens come first, and it gives a base model. Fine-tuning on examples teaches that model to follow requests, and rewards shape it further, as in Olmo 3's SFT, DPO and RLVR. Only a finished model can teach a student, and quantising then shrinks the student's file. The last tests and red-teaming come before release, and the model card reports them.

## What you can do now

- Name the stages that make a model, in order, from data to model card.
- Explain what fine-tuning, feedback training, distillation and quantisation each change.
- Follow an open model's family tree from its model card, stage by stage.
- Say why a fully open model can be checked at every stage and a closed one cannot.

---

[Back to Understanding models](README.md) · [Next: Size and memory](05-size-and-memory.md)
