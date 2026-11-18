# Planning loops

Advanced agents · Chapter 7 of 10 · 20 minutes

Plan-and-execute, ReWOO and LLMCompiler plan the work before they do it, and they differ in model calls, supersteps and time.

## Goals

- Run plan-and-execute: a plan, one step at a time, and a replan after each step.
- Run ReWOO: one plan with #E placeholders, the tools, then one answer.
- Run LLMCompiler: a task graph whose independent tasks run at the same time.
- Pass the check.

## The read

### Plan first, then act

The agents so far decided one tool call at a time: call the model, run the tool, call the model again. That is the ReAct loop. A planning agent asks the model for a plan first, then works through it. Three published patterns do this in different ways. The lab compares them on three counters: model calls, supersteps (one round of node runs, as in chapter 1) and how long a run takes.

### Plan-and-execute

A planner node writes a list of steps. An agent node does the first one: the model picks a tool call, and the node runs it. A replan node reads what is done, then answers or rewrites the steps left. The plan can change after every result, but each step costs at least two model calls: one to act, one to replan. The idea comes from the Plan-and-Solve paper (2023) and LangGraph's plan-and-execute tutorial.

### ReWOO

ReWOO (2023) writes the whole plan in one model call and names each result before it exists: #E1 = search[…], then #E3 = forecast[#E1]. #E1 is a placeholder. A tool node runs the steps in order and puts each result in for its name, with no model call. A solver node calls the model once more to answer. That is two model calls in all, but the plan cannot change once it is written.

### LLMCompiler

LLMCompiler (2023) writes the plan as a task graph without loops, a DAG: 3. forecast($1) means task 3 needs the result of task 1. A scheduler, the paper's task fetching unit, starts each task as soon as the tasks it needs are done, so tasks that need nothing from each other run at the same time. A joiner reads the results, then answers or asks for a new plan.

### In this lab

One research task runs three ways: Saturday's forecast where each of two trail races starts, for Halden, the fictional shop. Each graph is real LangGraph.js 1.5.1, drawn from getGraphAsync(). In Node you could also write the scheduler with the functional API, as an entrypoint whose task() calls run in parallel. task() needs AsyncLocalStorage, which the browser build lacks, so the schedule node here starts plain promises and passes config on by hand. The model is scripted, and each call takes a scripted time, sped up on screen and counted as simulated time.

## Try it: One task, three planners

Run the same research task as plan-and-execute, ReWOO and LLMCompiler, then compare their model calls, supersteps and simulated time.

https://agenticademy.org/en/paths/advanced-agents/07-planning-loops

## Check

### Question 1 of 3

A task needs five searches that do not depend on each other, then one answer. Which pattern can finish soonest?

- Plan-and-execute: replan keeps the plan up to date.
- ReWOO: it calls the model only twice.
- **✓** LLMCompiler: the five searches can run at the same time.
- All three take the same time, because the searches are the same.

**Why:** LLMCompiler's scheduler starts every task whose inputs are ready, so the five searches overlap and take about as long as the slowest one. ReWOO also calls the model only twice, but its tool node runs one search per superstep, one after another. Plan-and-execute adds two model calls per search. The searches are the same; when they run is not.

### Question 2 of 3

A ReWOO plan has the step #E3 = forecast[#E1]. What happens to #E1 before forecast runs?

- **✓** The tool node puts the result of step #E1 in its place.
- The model is called again to turn #E1 into a town.
- forecast receives the text "#E1" and looks it up.
- The solver fills it in at the end.

**Why:** #E1 is a placeholder for the result of the step named #E1. The tool node replaces it with that result before it calls forecast, so no model call is needed between steps. That is how ReWOO gets by with two model calls: one to plan, and one in the solver, which runs after every tool, too late to fill anything in.

### Question 3 of 3

The Lake Loop is cancelled, so the search for its start town returns "No result." Which pattern can drop the Lake Loop's forecast before it runs?

- **✓** Plan-and-execute: replan reads every result before the next step.
- ReWOO: the tool node skips a step whose input found nothing.
- LLMCompiler: the scheduler skips a task whose input found nothing.
- None of them: a plan cannot change once it is written.

**Why:** Plan-and-execute calls replan after every step, so it can drop the forecast from the steps left: that is what its extra model calls pay for. ReWOO's tool node only fills in results, so it runs forecast[#E2] with "No result." as the town. LLMCompiler's scheduler also runs forecast($2) as planned; only the joiner, after every task, can ask for a new plan.

## What you can do now

- Build plan-and-execute, ReWOO and LLMCompiler as LangGraph.js graphs.
- Count what each pattern costs in model calls, supersteps and time, and say where the difference comes from.
- Choose a pattern: replan when results can change the plan, plan once when they cannot, and run independent steps at the same time.

---

[Back to Advanced agents](README.md) · [Next: Spending compute](08-spending-compute.md)
