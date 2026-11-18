# A deep-agent harness

Advanced agents · Chapter 9 of 10 · 20 minutes

A deep agent plans with a todo list, keeps notes in virtual files, hands parts of a job to subagents with a fresh context, and pauses before the tools you name.

## Goals

- Start the job and watch the agent write and update its todo list.
- Approve the note writes and watch the file tree grow.
- Run three subagents in parallel and compare their context sizes.
- Pass the check.

## The read

### A harness for long jobs

A harness is the code around the model loop: the prompt, the tools and what each call sees. LangChain's open-source deepagents package is one, built on LangGraph. Its 2025 announcement named four parts: a detailed prompt, a planning tool, subagents and a file system.

### Todos and files

write\_todos({ todos }) takes the whole list each time, each item pending, in\_progress or completed, and keeps it in state.todos. It does nothing else: the gain is that the model rereads its plan on every call. Two calls in one turn are refused. Turn it on by adding LangChain's todoListMiddleware() to middleware.

The file tools (ls, read\_file, write\_file, edit\_file, glob, grep) work on state.files, a virtual file system in the graph's state. Subagents can read notes there, and they outlast a summary of old messages.

### Subagents

task({ description, subagent\_type }) starts a subagent with a fresh context: its own system prompt and the description, none of the caller's messages. It starts from the caller's files, the files it writes are merged back, and only its last message returns, as the tool result. Calls in one turn run in parallel. A general-purpose subagent with the caller's tools is there by default.

### Skills, memory and approvals

A skill is a folder with a SKILL.md file. With skills: ['/skills/'], the prompt lists each skill's name and description, and the agent reads the file only when a task needs it. The memory option loads AGENTS.md files into the prompt. interruptOn, with a checkpointer, names tools that pause the run until a person approves, edits or rejects each call. This page pauses at a breakpoint (interruptBefore), because interrupt() needs AsyncLocalStorage.

### Less scaffolding

Every part costs tokens on every call. deepagents 1.12 (2026-07-29) made the todo list opt-in and left its generic system prompt blank: current models need less guidance. Anthropic's post on harness design (2026-03-24) agrees: with each new model, remove the parts it no longer needs.

### In this lab

A re-created harness writes a brief on refund complaints at Halden, a fictional shop. Unlike deepagents 1.16, it turns the list on with todos: true, shows the skills to the researcher without a skills list of its own, keeps the old generic prompt line and has no AGENTS.md memory. The lead pauses before write\_file and task so you can follow it; in your own agent, pause before tools that change things outside it.

## Try it: A research job, one part at a time

Start the job and watch the todo list, let the lead write its notes, then approve three subagents and compare their context sizes.

https://agenticademy.org/en/paths/advanced-agents/09-deep-agent-harness

## Check

The lead's context is about 1,400 tokens when its model makes this call. The researcher reads about 600 tokens of files, edits the note, and ends with a 40-token report. What changes in the lead's state?

Code:

```
const lead = createDeepAgent({
  systemPrompt: 'You write briefs.',
  subagents: [{
    name: 'researcher',
    description: 'Researches a cause.',
    systemPrompt: 'Edit the note, then report.',
  }],
});

// The lead's model asks for:
task({
  subagent_type: 'researcher',
  description:
    'Late refunds: /notes/late-refund.md',
})
```

- Its messages gain the researcher's whole conversation, about 650 tokens; its files stay the same.
- Its messages gain one tool result, the 40-token report; its files stay the same, because the researcher edited a copy.
- **✓** Its messages gain one tool result, the 40-token report, and the edited note is merged into its files.
- Nothing changes until the lead reads /notes/late-refund.md itself.

**Why:** task starts the researcher with a fresh context: its own system prompt and the description, none of the lead's messages. What it reads stays in its own context. Only its last message returns, as the task tool's result, so the lead grows by about 40 tokens, and the files it wrote are merged into the lead's state.files. In the lab, three researchers read thousands of tokens while the lead grew by three short reports.

## What you can do now

- Turn on a deep agent's todo list by adding todoListMiddleware() to its middleware, and read the plan from state.todos.
- Keep notes and drafts in state.files with ls, read\_file, write\_file, edit\_file, glob and grep.
- Hand self-contained work to task subagents, and use the reports and files that come back.
- Pause a deep agent before chosen tools with interruptOn and a checkpointer, and drop scaffolding a newer model no longer needs.

---

[Back to Advanced agents](README.md) · [Next: Long-horizon agents](10-long-horizon.md)
