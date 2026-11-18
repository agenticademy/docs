# Comparing models

Understanding models · Chapter 7 of 8 · 15 minutes

Benchmarks and leaderboards help you compare models, but margins of error, effort settings, cost, leaked tests, maxed-out tests, the test program and who ran it all change what a score means.

## Goals

- Read a leaderboard with its intervals and find the ties.
- Compare one model at five effort settings with a switch to another model.
- Match four benchmark traps to examples.
- Pass the check.

## The read

### Benchmarks and leaderboards

A benchmark is a fixed set of test tasks with a way to score the answers. A leaderboard ranks models by such scores. The visual uses two real ones. On Arena, people send a prompt, read two answers from models they cannot see, and vote for the one they prefer; millions of votes become a score for each model. Artificial Analysis, an independent company, runs nine benchmarks itself and combines them into one number, its Intelligence Index.

### Every score has a margin

A score from votes, or from a sample of tasks, is an estimate. Arena writes it as a score ± a margin: the true score most likely lies in that range, called a 95% interval. Fewer votes give a wider range. When two models' ranges overlap, the board cannot tell them apart, so treat them as tied, whatever their rank.

### Settings and cost

Many models have an effort setting, such as low, medium, high or max: how much the model may think before it answers, as in chapter 3. More effort often scores higher, but the model writes more tokens, so each task costs more and takes longer. Artificial Analysis also shows a cost per task: what running the model on one of its tasks cost, in US dollars. A score means little without its setting and its cost.

### Traps

Contamination: the test's questions and answers leaked into the training data, so a model may remember them instead of solving them. Saturation: the top models all score near the maximum, so the test no longer separates them. The harness is the program that runs the test, with its instructions, tools and retries; a better harness can raise the score of the same model. A maker's own score is run its own way and can differ from an independent one. Version drift: tests change between versions, so a score needs its version and its date.

### In the visual

The Arena and Artificial Analysis figures are real, as published, in a snapshot dated 2026-11-03. The chart's cost axis is a log scale: $0.10 to $1 takes the same width as $1 to $10, so cheap and costly models both fit. The examples in the last step are made up.

## Try it: Reading benchmark scores

Find the ties on a real leaderboard, climb one model's effort ladder, then match four traps to made-up examples.

https://agenticademy.org/en/paths/understanding-models/07-comparing-models

## Check

### Question 1 of 3

In an example, model A scores 1,310 ± 8 on a leaderboard and model B scores 1,302 ± 9. What can you say?

- A is clearly better than B.
- **✓** Their intervals overlap, so the leaderboard cannot tell them apart.
- B is better, because its interval is wider.
- The scores must be wrong.

**Why:** A's interval runs from 1,302 to 1,318, and B's from 1,293 to 1,311. The ranges overlap, so the gap may be noise: treat them as tied, not A as clearly better. A wider interval means less certainty, for example fewer votes, not a better model. And nothing says the scores are wrong: they are estimates with their margins.

### Question 2 of 3

A report says model X scores 5 points higher than model Y on an index. What should you ask before you switch to X?

- Nothing: 5 points is a clear win.
- **✓** Which effort setting each one ran at, and what each cost per task.
- Which of the two makers is larger.
- Which model came out first.

**Why:** In the visual, low to max moved each model with five settings by 20 to 29 points, at about 8 to 20 times the cost per task. So a 5-point gap can come from the settings alone, and the higher score may cost far more per task. The size of the maker and the release date say nothing about either.

### Question 3 of 3

A blog post puts model A's score on the current version of a test next to model B's score on the version from a year ago. A's number is higher. What can you conclude?

- A is better, because its number is higher.
- B is better, because it was tested first.
- **✓** Not much yet: different versions use different tasks, so the two numbers are not on the same scale.
- The newer version must be easier.

**Why:** That is version drift. A new version changes the tasks, how many there are or how they are weighted, so its scores are not on the old scale. On 2026-11-03, for example, Artificial Analysis's index was at version 5.0.1, with nine evaluations. Compare models on the same version, read on the same date. Testing first does not make a model better, and nothing says the newer version is easier.

## What you can do now

- Read a leaderboard's intervals and say which models it cannot tell apart.
- Compare models at named effort settings, together with their cost per task.
- Spot contamination, saturation, harness effects and a maker's own scores in a claim.
- Ask for the version, the date and who ran the test before trusting a score.

---

[Back to Understanding models](README.md) · [Next: Choosing a model for an agent](08-choosing-for-an-agent.md)
