# Plans, and agents working together

Agents for everyone · Chapter 5 of 8 · 14 minutes

Big jobs are split into steps, and sometimes shared among several agents led by one.

## Goals

- Pick the steps that can run at the same time.
- Run the job with one agent, then with a team.
- Pass the check.

## The read

### A plan is a list of steps

Give an agent a big goal, such as organising a team lunch, and it first makes a plan: it breaks the goal into smaller steps. Some steps depend on others. You cannot book a restaurant before you know the day. Other steps are independent and can happen at the same time. Steps that run at the same time are said to run in parallel.

### One agent, or a team

A single agent can work through the whole plan by itself, one step after another. Everything it reads along the way, every reply and every result, piles up in its context, the text the model reads at each step (chapter 4). A long job can fill it.

Another way is a team of agents. A lead agent, called an orchestrator, keeps the plan and hands each part to a specialist: another agent with its own instructions, its own tools and its own context. Passing a part of the work from one agent to another is called a hand-off. The specialist does its part and sends back a short result. The orchestrator never reads the details, only the summaries.

### What a team costs

A team can finish sooner, because independent steps run in parallel, and each agent's context stays small and focused on one thing. But every agent makes its own model calls, so a team usually costs more to run. And every hand-off is one more place where a detail can be lost or misread.

A team is worth it when a job has large parts that do not depend on each other. For a small job, one agent is simpler and cheaper.

### Words for this chapter

Plan: the list of steps toward a goal. In parallel: at the same time. Orchestrator: the lead agent that splits the work and combines the results. Specialist: an agent given one part of the work. Hand-off: passing a part of the work, with a short brief, from one agent to another.

## Try it: One agent or a team

Mark the steps that can start straight away, at the same time. Then run the same job with one agent and with a team, and compare the scorecards.

https://agenticademy.org/en/paths/agents-for-everyone/05-plans-and-teams

## Check

### Question 1 of 3

What does an orchestrator agent do?

- It does every step itself.
- **✓** It splits the work, hands parts to specialist agents and combines their results.
- It checks that the internet is working.

**Why:** An orchestrator leads: it plans, hands parts of the work to specialists and puts their results together. A single agent doing every step itself is the other way of working.

### Question 2 of 3

When does a team of agents help most?

- **✓** When the job has large parts that do not depend on each other.
- For a one-step question.
- Always: more agents are always better.

**Why:** Independent parts can run in parallel, each with a small, focused context. A one-step job gains nothing from a team and still pays for the hand-offs.

### Question 3 of 3

Which is a real cost of using several agents?

- They cannot use tools.
- **✓** More model calls, and more hand-offs where something can go wrong.
- They forget the goal by design.

**Why:** Each agent makes its own model calls, so the bill grows, and each hand-off can lose or garble a detail. In the scorecard, the team made more than twice as many calls.

## What you can do now

- Explain what a plan is and why some steps can run in parallel.
- Describe an orchestrator and its specialist agents.
- Weigh one agent against a team for a given job.

---

[Back to Agents for everyone](README.md) · [Next: Standards, in plain words](06-standards.md)
