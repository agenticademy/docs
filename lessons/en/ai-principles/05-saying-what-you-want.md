# Saying what you want

AI principles · Chapter 5 of 8 · 15 minutes

Where a request leaves a gap, the model often fills it with a likely guess, so a good request describes the result, the way to work and the behaviour you want, with the facts and an example only you can give.

## Goals

- Read a vague request and the answer it got, and mark what the answer had to guess.
- Add parts to the request one at a time until the checker finds no gaps.
- Compare the answers to the vague and the full request.
- Pass the check.

## The read

### A gap gets a guess

Chapter 4 was about what to hand over. This one is about how to ask. What you type to an AI chat assistant is a prompt: the text the model starts from, as in chapter 2. In a plain chat, with no access to your files or calendar, all the model knows about your situation is what you write.

Where your request leaves a gap, the model often fills it with what is likely in general, the way it picks likely words. The result reads well, so the guesses are easy to miss: a date nobody set, a promise nobody made.

### Three things to describe

Write your request as a brief: the instructions you would give a capable helper who knows nothing about your situation. Three things are worth describing.

The result: who it is for, its form (a list, a notice, an email) and how long it should be.

The way to work: the steps to follow, and what to ask you before starting. “If a fact is missing, ask me” turns a guess into a question.

The behaviour: how it should sound, and what to avoid, such as promises you cannot keep.

### What only you know

Two more parts carry what the model cannot guess. Context is the background facts: dates, names, what has already been decided. An example shows what good looks like, such as a notice that worked last time. Say what to take from it, such as its layout, or the model may copy its old date too.

Words like “make it good” add length, not information: they do not say what good means.

### In the visual

You start from a vague request for a community garden notice and the answer it got, and mark what that answer had to guess. Then you add parts one at a time while a checker flags what is still missing. The checker is a few fixed rules that run in this page and look at which parts your request has; it is not an AI. Last, you send the full request and compare the two answers. The garden and both answers were written for this chapter to show the kind of guesses a model can make.

## Try it: Brief builder

Mark what a vague request left to guess, add parts until the checker finds no gaps, then compare the two answers.

https://agenticademy.org/en/paths/ai-principles/05-saying-what-you-want

## Check

### Question 1 of 4

You ask an AI chat assistant, which has no access to your calendar or email: “Write a reminder about Friday's team lunch.” Its answer says the lunch is at 12:30 in the main hall. You never gave a time or a place. What happened?

- It found the lunch in your calendar and copied the time and the place from there.
- It knows when and where team lunches happen at your workplace, from its training.
- **✓** Your request left gaps, and the model filled them with likely details.

**Why:** The model knew only what you wrote. Where it left a gap it wrote what is likely in general, which reads well and may be wrong. It could not look anything up, and it knows nothing about your workplace unless you tell it.

### Question 2 of 4

You want help planning a birthday dinner for your father. Which line, added to your request, gives the AI the most it can act on?

- **✓** “Eight people, one of them vegetarian, about 30 euros each, near the station. He likes quiet places.”
- “Make it really special, the kind of evening he will remember for years.”
- “Be as creative as you can, and surprise me with ideas I would not think of.”

**Why:** The first gives facts the AI cannot guess: how many people, who eats what, the budget, the area and what he likes. “Special” and “creative” sound helpful but do not say what you want, so the AI is left to guess again.

### Question 3 of 4

Which line describes the way to work, rather than the result or the behaviour?

- “Keep it under 100 words, as a short list that fits on one phone screen.”
- **✓** “First list the facts you will use, and ask me about any that are missing.”
- “Use a warm, friendly tone, and do not promise anything on my behalf.”

**Why:** Listing the facts first and asking about gaps is a way to work through the task. The length and the form belong to the result; the tone and what to avoid belong to the behaviour. All three are worth saying; they answer different questions.

### Question 4 of 4

You paste last year's school trip letter as an example and ask for this year's. The new letter has last year's date and price. What should your request have said?

- “Make it better than last year's letter, and more up to date.”
- Nothing more: an example on its own shows the model which parts to keep.
- **✓** “Copy its layout, not its facts. This year's date and price are below.”

**Why:** An example shows what good looks like, but not which parts to keep. Say what to take from it, here the layout, and give the new facts. “Better” and “up to date” do not say what to change, and an example on its own can be copied whole, old facts included.

## What you can do now

- Explain why a vague request gets an answer full of guesses.
- Describe the result, the way to work and the behaviour you want from an AI.
- Add the context and an example only you can give, and say what to take from the example.

---

[Back to AI principles](README.md) · [Next: Checking what you get](06-checking-what-you-get.md)
