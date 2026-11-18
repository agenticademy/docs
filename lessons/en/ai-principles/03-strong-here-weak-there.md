# Strong here, weak there

AI principles · Chapter 3 of 8 · 15 minutes

AI does some hard tasks well and fails at easy-looking ones, can state false things with confidence, and is more useful when it says it does not know.

## Goals

- Answer six questions or say “I don't know”, then score them two ways.
- Sort six everyday tasks onto a dated map of where AI does well and where it fails.
- Pass the check.

## The read

### Uneven, and not by difficulty

In chapter 2 you saw a model write by picking likely next pieces of text, learned from the examples it was trained on. What it does well depends on those examples, not on what people find hard. It can translate a sign into three languages in seconds, then miscount the letters in a word, because it reads pieces of words rather than single letters.

Researchers call this a jagged frontier: the edge between what AI does well and what it does badly zigzags. The phrase comes from a 2023 study in which consultants used AI on realistic tasks (Dell'Acqua and colleagues). On tasks inside the frontier their work got better. On a task just outside it, those using AI were right less often than those working without it. The frontier also moves as models change, so any map of it needs a date.

### Confident and wrong

A model always produces an answer, and its words sound just as sure when it is wrong. Writing something false in a confident tone, such as a quote from a book that does not exist, is called confabulation; many people call it hallucination. Being calibrated means your confidence matches how often you are right: when you say you are sure, you nearly always are.

### Why guessing pays on most tests

Abstaining means saying “I don't know” instead of answering. Whether it pays depends on the scoring rule, the way a test turns answers into points. If only right answers count, a guess can only help, so someone who always answers beats someone who admits doubt. A 2025 paper by researchers at OpenAI argues that most tests used to rank AI models score this way, so a model that guesses ranks above one that admits it is unsure. The fix it proposes: make a wrong answer cost more than “I don't know”.

### In the visual

You answer six questions; nobody could know some of the answers from this page. Your answers are scored both ways, next to two simulated players. Then you sort six everyday tasks onto a dated map: usually fine, check carefully, or don't rely on it.

## Try it: Answer or abstain, then map the tasks

Answer six questions, see them scored two ways, then sort six everyday tasks onto a dated map.

https://agenticademy.org/en/paths/ai-principles/03-strong-here-weak-there

## Check

### Question 1 of 3

A quiz gives 1 point for a right answer, 0 for “I don't know” and minus 1 for a wrong answer. You are about 30% sure of your answer. What scores best on average?

- Answer anyway: a guess is worth a try under any scoring.
- **✓** Say “I don't know”: at 30% sure, a guess loses points on average.
- Either one: both score the same on average.
- Answer, and add a longer explanation to earn more.

**Why:** Answering earns 0.3 × 1 + 0.7 × (−1) = −0.4 points on average, and “I don't know” earns 0, so the two are not the same and abstaining scores best. A guess is only worth a try when wrong answers cost nothing, and a longer explanation does not change the points. The same holds for a model scored this way.

### Question 2 of 3

A chat assistant translates a long letter into Japanese well, then gets the number of letters in a short word wrong. What does this show?

- It is broken, so none of its work can be trusted.
- Counting letters is harder than translating, for people too.
- It did not try hard on the easy task, as a bored person might.
- **✓** What it does well does not follow what people find hard.

**Why:** This is the jagged frontier: a model can do a task that is hard for people well and slip on one a child can do, because it reads pieces of words, not letters. So judge each kind of task on its own. One slip does not make all its work untrustworthy, counting is not harder for people, and it was not slacking: the slip comes from how it reads text.

### Question 3 of 3

You ask a chat assistant with no web search whether the bakery on your street is open right now. It replies: “Yes, it is open until 6 pm today.” What should you do?

- **✓** Treat it as a guess and check the bakery's own page, or call.
- Trust it: it sounds sure, so it must know.
- Ask again: if it says the same thing twice, it is right.
- Trust it, because the answer is short and specific.

**Why:** It has no way to see today's hours, so a sure and specific answer can still be confabulation: a model's words sound the same whether it knows or not. Asking again only gets another guess, which may or may not match. Check a source that does know.

## What you can do now

- Explain why AI can do a hard task well and fail at an easy-looking one.
- Say why a confident answer can still be wrong, and why scoring only right answers rewards guessing.
- For an everyday task, decide whether AI output is usually fine, needs checking, or should not be relied on.

---

[Back to AI principles](README.md) · [Next: What to hand over](04-what-to-hand-over.md)
