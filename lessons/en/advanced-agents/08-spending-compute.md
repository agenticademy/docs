# Spending compute

Advanced agents · Chapter 8 of 10 · 20 minutes

Evaluator-optimizer loops, reflection, best-of-n and tree search spend more model calls to get a better answer, and each needs a budget.

## Goals

- Run a generator and judge loop with a rubric and a round cap.
- Generate several answers and pick one by vote.
- Search a small tree of plans under an expansion budget.
- Pass the check.

## The read

### More calls for a better answer

One model call gives one answer. More calls can give a better one: a judge grades drafts, several samples are compared, or a search looks ahead. Each pattern multiplies the calls, the cost and the wait, so each needs a budget that stops it.

### A generator and a judge

An evaluator-optimizer loop has two roles. The generator writes a draft. The judge is a separate model call with its own prompt: it scores the draft against a rubric, a short list of criteria each scored on a scale, and writes notes for the next draft. A threshold says when a draft passes, such as every criterion 4 or more out of 5. A round cap bounds the loop: when it is reached, the loop returns the best draft so far, marked as not passed.

Reflection, or Self-Refine, is the same loop with the model critiquing its own draft. A separate judge is easier to keep strict and to test. As in Agent systems chapter 8, compare its scores with people's before you trust it.

### Best-of-n and voting

Best-of-n asks for n answers to the same prompt, sampled at a temperature above 0 so they differ, and keeps one: a judge picks the best, or a vote keeps the most common final answer. Voting on final answers is called self-consistency; it suits answers you can compare exactly, such as an amount. In LangGraph the samples are n Sends in one superstep: n calls that run at once, so you wait only for the slowest.

### Tree search

A tree search plans steps ahead. It expands a step into possible next steps, gives each a value estimate, how promising it looks, and keeps expanding the most promising open step. Language Agent Tree Search (LATS, 2023) does this with Monte Carlo tree search, using the model to propose steps, estimate values and reflect on failures. An expansion budget caps how many steps it expands. The lab runs a smaller best-first search over two levels, with simulated values.

### When it is not worth it

Extra calls pay off when a better answer matters and something can tell better from worse: a rubric, an exact answer to vote on, a value to estimate. If one call already passes, or nothing can check quality, they only add cost and time. Measure it: run your evaluation cases with and without the pattern.

## Try it: Three ways to spend more calls

Halden's ticket T-48213, three ways: a judge loop polishes the reply, a vote picks the refund amount, and a tree search plans the steps. Each stops at its budget.

https://agenticademy.org/en/paths/advanced-agents/08-spending-compute

## Check

What does this run return, and after how many model calls (one per draft, one per judgement)?

Code:

```
const CAP = 3;
const PASS = 4; // per criterion, of 5
const scores = (r) =>
  Object.values(r.scores);
const passes = (r) =>
  scores(r).every((s) => s >= PASS);
const total = (r) =>
  scores(r).reduce((a, b) => a + b);

function review(state) {
  const last = state.rounds.at(-1);
  if (passes(last)) return 'finish';
  if (state.rounds.length >= CAP)
    return 'finish';
  return 'generate';
}

function finish(state) {
  const last = state.rounds.at(-1);
  const best = passes(last) ? last
    : state.rounds.reduce((b, r) =>
        (total(r) > total(b) ? r : b));
  return {
    answer: best.draft,
    passed: passes(best),
  };
}

// The judge's scores, round by round:
// 1: accurate 3, complete 2, tone 4
// 2: accurate 4, complete 4, tone 3
// 3: accurate 4, complete 3, tone 3
```

- Draft 3, marked as passed, after 6 calls.
- **✓** Draft 2, marked as not passed, after 6 calls.
- Draft 3, marked as not passed, after 6 calls.
- Nothing yet: a fourth round runs, because no draft has passed.

**Why:** No round passes: round 1 has complete 2, round 2 has tone 3, round 3 has complete 3 and tone 3. So review sends the loop back until rounds.length reaches CAP, after round 3: three drafts and three judgements, 6 calls, and no fourth round. finish then keeps the best total, draft 2 with 11 over draft 3 with 10, because a revision can get worse; the last draft is not always the best. passed is false, so the caller can flag the draft or hand it to a person.

## What you can do now

- Build an evaluator-optimizer loop with a separate judge, a rubric, a pass threshold and a round cap.
- Run best-of-n samples as Sends in one superstep and keep the answer most of them agree on.
- Explain how a tree search expands, scores and backs up values under an expansion budget.
- Decide when extra calls are worth their cost, and measure it with your evaluation cases.

---

[Back to Advanced agents](README.md) · [Next: A deep-agent harness](09-deep-agent-harness.md)
