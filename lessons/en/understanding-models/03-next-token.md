# Choosing the next token

Understanding models · Chapter 3 of 8 · 14 minutes

A model scores every possible next token, turns the scores into chances and picks one at random; temperature, top-k and top-p change the pick, and base, instruct and reasoning models are trained to write differently.

## Goals

- See the 20 likeliest next tokens for a prompt.
- Move temperature and top-p, and pick again.
- Ask a base model and an instruct model the same question.
- Pass the check.

## The read

### One token at a time

In the last chapter, text became tokens. A model writes the same way: it reads all the text so far, picks one next token, adds it to the text and starts again. A reply of 100 tokens takes 100 of these steps.

### Scores become chances

At each step the model gives every token it knows a score, called a logit. SmolLM2-360M, the small open model in this chapter, knows 49,152 tokens, so it gives 49,152 scores. The scores are then turned into chances, also called probabilities, that add up to 100%. A higher score means a larger chance: one point more means about 2.7 times the chance.

### A random pick, with settings

Then one token is picked at random, by those chances, like a draw from a bag where likely tokens have more tickets. So the same prompt can get different replies. Three settings change the draw. Temperature divides every score before it becomes a chance: below 1 the likely tokens get even likelier, above 1 the chances even out. Top-k keeps only the k likeliest tokens. Top-p keeps the likeliest tokens until their chances add up to p, so a top-p of 0.9 drops the long tail of unlikely ones. Whatever is kept is scaled back up to 100%.

### Base and instruct models

A base model has only learned to continue text, so a question may get more questions. An instruct model was trained further on conversations, written in a chat format that marks who is speaking. It answers, then writes an end token, a special token that means it is done.

### Reasoning models

A reasoning model writes its working first, in the same token-by-token way, and then the answer. As of 2026-11-03, closed models such as Claude Opus 5.5 and GPT-6.1 Sol make reasoning a setting, and open ones such as DeepSeek-R1 and Olmo 3 7B Think ship as separate reasoning models.

### In the visual

The numbers in the visual were recorded from SmolLM2-360M and its instruct version, both from Hugging Face. If your browser has WebGPU, a way for web pages to use the graphics card, you can also download the instruct model and run it live.

## Try it: Next-token sampler

Choose a prompt and read the chances, then shape them and pick, one token at a time. Last, ask the base and the instruct model the same question.

https://agenticademy.org/en/paths/understanding-models/03-next-token

## Check

You pick the next token 100 times with these settings. What do you get?

The settings:

```
Prompt: The capital of France is
Model:  SmolLM2-360M (base)

Next token   Chance
Paris        20.3%
the          19.6%
located      10.5%
situated      4.7%
called        3.7%
… 15 more, then all the rest

Temperature: 1
Top-k:       off
Top-p:       0.5
```

- Paris all 100 times: it is the likeliest token.
- Paris and the, about 50 times each.
- **✓** Paris, the and located, about 40, 39 and 21 times.
- Any of the 49,152 tokens, each as often as its chance says.

**Why:** Top-p keeps the likeliest tokens until their chances reach p. Paris and the add up to 39.9%, short of 50%; with located the total is 50.4%, so those three stay and the rest are cut. The three are scaled back up to 100%: about 40%, 39% and 21%. Paris every time would need a setting that keeps one token, such as top-k at 1 or top-p at 0.2, and every token could come up only with top-p at 1.

## What you can do now

- Explain how a model turns scores into chances and picks the next token.
- Say what temperature, top-k and top-p do to the chances.
- Tell a base model's output from an instruct model's, and say what a reasoning model adds.

---

[Back to Understanding models](README.md) · [Next: Where models come from](04-where-models-come-from.md)
