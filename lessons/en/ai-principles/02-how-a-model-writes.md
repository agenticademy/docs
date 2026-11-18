# How a model writes

AI principles · Chapter 2 of 8 · 14 minutes

A language model writes one token at a time by drawing from a list of chances, temperature changes how it draws, and nothing in that step checks whether the words are true.

## Goals

- Read the chances for a prompt and let the model take the likeliest word.
- Pick the next five words yourself.
- Write at a low and a high temperature and watch the text drift.
- Pass the check.

## The read

### One piece at a time

In the last chapter a classifier learned from examples to sort points into kinds. A language model, the kind of AI behind chat assistants, learned from a vast amount of text to do one thing: given some text, say what is likely to come next. It writes by repeating that step. It works out a chance for every possible next piece, picks one, adds it to the text and starts again.

### Tokens and chances

The pieces are called tokens. A token is a short piece of text: a word, part of a word or a punctuation mark. A chance is a number from 0% to 100%, and the chances of all possible next tokens add up to 100%. After “The café opens at 8 in the”, the token “morning” gets a high chance and “moon” a tiny one. The text you start from is called the prompt.

### Temperature

The model draws the next token at random, weighted by the chances, so likely tokens come up most often. Temperature is a setting that reshapes the chances before the draw. Near 0, the likeliest token nearly always wins, and the same prompt gives nearly the same text each time. Higher up, unlikely tokens get picked more often: the text varies more and, past a point, drifts into nonsense.

### Fluent is not the same as true

Every token is picked because it is likely to come next, not because it is true. Nothing in this step checks a fact. A model can write a smooth sentence with a wrong date, price or name, and a low temperature does not fix that: it only picks the likeliest words. Checking what it writes stays your job, and a later chapter shows how.

### The model in this page

The explorer below runs a tiny model that your browser trains when the page opens, on ten of Aesop's fables. Its tokens are whole words and punctuation marks. It looks only at the last two and counts what came after them in the fables. The models behind chat assistants use tokens that can be parts of words, take in far more of the text and have billions of parameters, the numbers they adjust while learning. The choosing step works the same way.

## Try it: Next-word explorer

Read the chances for a prompt, pick words yourself, then move the temperature and watch the text drift.

https://agenticademy.org/en/paths/ai-principles/02-how-a-model-writes

## Check

### Question 1 of 3

You type “Our train to the coast leaves at” and a model continues the text. What does it do?

- It looks up the timetable and copies the time.
- **✓** It gives each possible next token a chance, draws one, adds it, then does the same again.
- It writes the whole answer at once, then checks it against the facts.
- It always picks the most surprising token, to keep the text interesting.

**Why:** A language model writes one token at a time, drawing each from chances. It does not look anything up unless it is connected to a tool, it does not write everything at once or check facts, and likely tokens are picked most often, not surprising ones.

### Question 2 of 3

You ask a model the same question twice with a high temperature. What should you expect?

- Exactly the same answer both times.
- **✓** Answers that may differ, because each token is drawn at random from reshaped chances.
- An error, because a high temperature is not allowed.
- More accurate answers, because the model tries more options.

**Why:** A high temperature evens out the chances, so each draw can go a different way and two answers can differ. Near 0 they would be nearly the same. A high temperature is an allowed setting, and it does not make answers more accurate: it only makes unlikely tokens more common.

### Question 3 of 3

A model writes, smoothly and without hedging: “The café on Elm Street opens at 7:30 on Sundays.” What does the smooth wording tell you about the time?

- It is right, or the model would not write it so smoothly.
- It is right if the temperature was low.
- **✓** Nothing for sure: the words were picked because they were likely, so check the time with the café.
- It is wrong, because models always make up times.

**Why:** Smooth wording shows the tokens were likely, not that they are true. A low temperature only makes the model pick the likeliest tokens, which can still be wrong, and models are not always wrong either. Check facts like opening times against a source you trust.

## What you can do now

- Explain that a language model writes one token at a time, drawing each from chances.
- Say what temperature changes, and what it does not.
- Explain why a smooth, confident answer can still be wrong.

---

[Back to AI principles](README.md) · [Next: Strong here, weak there](03-strong-here-weak-there.md)
