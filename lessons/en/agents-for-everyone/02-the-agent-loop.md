# The loop: look, decide, act

Agents for everyone · Chapter 2 of 8 · 12 minutes

Every agent runs the same loop: it looks at what it knows, decides the next step, acts, and looks again.

## Goals

- Make three decisions as if you were the model.
- Bring the agent to its goal.
- Put the steps of one turn in order.

## The read

### One loop, again and again

In chapter 1 you saw an agent work in steps. Each step follows the same loop of three parts.

Look: the agent gathers what it has so far, which is your request and the result of its last step. Decide: the model reads all of that and chooses the next step, or decides that the goal is reached. Act: the agent takes the step, usually by using a tool, and a result comes back. Then it looks again.

One pass through look, decide and act is called a turn. A small job can take three or four turns; a large one can take dozens.

### Why it looks again

What a tool returns is new information. If a search shows that one plumber is away all month, the next decision can take that into account. This is how an agent changes course without being told: it reads the result before it chooses again.

### When it stops

An agent stops for one of three reasons. The goal is reached. It needs you: it asks a question or asks for your approval, and waits. Or it hits a limit, such as a maximum number of turns.

### How loops go wrong

A poor decision costs a turn. Booking before anyone has been found, for example, achieves nothing. Worse is a runaway loop: the agent repeats the same step, gets the same result, and repeats it again. Without a limit it could go on for ever, and every turn uses the model, which takes time and often costs money. That is why real agents count their turns and stop at a limit.

### Your turn

Below, you play the model. The agent looks, then you decide, then the agent acts and shows you what came back. Try a poor choice or two, and see what they cost.

## Try it: You are the model

You decide each step for the agent. Watch what each action returns, and see how a poor choice costs an extra turn.

https://agenticademy.org/en/paths/agents-for-everyone/02-the-agent-loop

## Check

Put the parts of one turn of the loop in order.

1. Read the request and the result of the last step.
2. Decide the next step, or decide the goal is reached.
3. Use a tool to take that step.
4. Read what the tool returned.

**Why:** Look, decide, act, then look again: what the tool returns becomes part of what the agent looks at in the next turn.

## What you can do now

- Describe the loop every agent runs: look, decide, act.
- Name three reasons an agent stops: goal reached, it needs you, or a limit.
- Explain why agents need a limit on how many steps they take.

---

[Back to Agents for everyone](README.md) · [Next: Tools: how an agent reaches the world](03-tools.md)
