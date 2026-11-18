# What AI is, and what it is not

AI principles · Chapter 1 of 8 · 12 minutes

Most AI systems in use today learn patterns from examples and use them to infer an output for something new. Such a system is not a mind, a database or a search engine, and its examples shape every answer.

## Goals

- Place examples of two kinds and let the classifier learn them.
- Test a new fruit and see which examples decide.
- Add a skewed batch of examples and watch the boundary move.
- Pass the check.

## The read

### Learning from examples

Most AI systems in use today are programs that learn patterns from examples, then use those patterns to produce an output for something new. The examples are called data. The output can be a label such as “spam”, a number such as next week’s sales, or the next word of a sentence.

Working out an output this way is called inference: the answer comes from patterns, not from looking it up.

### Training, then use

Such a system has two phases. In training, it is shown many examples and adjusts itself to fit them. In use, it applies what it learned to inputs it has never seen. Using it does not change what it learned; that takes another round of training.

### What it is not

It is not a mind: it computes outputs from patterns and wants nothing. It is not a database: a database gives back only what was stored, while an AI system answers for inputs nobody stored, and can be wrong. It is not a search engine: it does not fetch pages, although some tools connect it to one.

### The examples decide

A system learns only from its examples, so they shape every answer. If the examples are skewed, meaning one kind or one source has far more of them than the rest, the answers lean the same way. A system that learned mostly from photos taken in one country tends to do worse on photos from elsewhere.

### In the visual

You train a classifier, a program that sorts things into kinds, to tell lemons from oranges by size and colour. Each fruit is a dot on a map. To judge a new fruit, it finds the 3 examples closest to it on the map, its nearest neighbours, and answers with the kind most of them are. The line where its answer switches from one kind to the other is its boundary.

For this simple classifier, training means keeping the examples. Large models instead adjust a huge number of internal values while they train, and keep those values rather than the examples.

## Try it: Teach a tiny classifier

Place lemons and oranges on the map, test a fruit it has not seen, then add a skewed batch and watch the boundary move.

https://agenticademy.org/en/paths/ai-principles/01-what-ai-is

## Check

### Question 1 of 3

A system was trained on thousands of photos labelled “cat” or “dog”. Now it labels a photo it has never seen. What is it doing?

- Looking the photo up in a store of answers.
- **✓** Inferring a label from patterns in its examples.
- Searching the web for the same photo.
- Thinking about the photo the way a person would.

**Why:** It applies patterns from its labelled examples to a new input: that is inference. A store of answers could only give back photos it already had, a web search finds pages that exist, and the system has patterns, not a person’s thoughts.

### Question 2 of 3

A tool that sorts a support inbox learned from many billing messages and only a few delivery messages. A new message is unclear: it could be either. What is the likely result?

- **✓** It is more likely to call it a billing message.
- It is equally likely to call it either, because it saw both kinds.
- It refuses to sort the message.

**Why:** Its examples lean towards billing, so near the boundary its answers lean that way too, as the batch of oranges pulled the boundary in the visual. It saw deliveries, but too few to hold their ground. And a classifier like the one in the visual always picks one of its kinds: it has no way to refuse.

### Question 3 of 3

In the visual, why did the classifier start calling a typical lemon an orange?

- The lemon changed size and colour.
- **✓** The batch put oranges closest to it, so most of its nearest neighbours became oranges.
- Someone told the classifier the right answer was orange.
- More examples always make a classifier less accurate.

**Why:** The lemon and the rule (the 3 nearest decide) stayed the same; only the examples changed. Nobody told it the answer. More examples are not the problem in themselves: lopsided ones are.

## What you can do now

- Explain in plain words that an AI system infers outputs from patterns in its examples.
- Tell training (when a system learns from its examples) apart from use (when it applies what it learned).
- Say why an AI system is not a mind, a database or a search engine.
- Explain why skewed examples lead to skewed answers.

---

[Back to AI principles](README.md) · [Next: How a model writes](02-how-a-model-writes.md)
