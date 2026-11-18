# Tools and tool calls

Build your first agents · Chapter 5 of 8 · 20 minutes

Defining tools with zod, binding them to a model, and handling the tool calls the model makes.

## Goals

- Add a described field to a tool's schema and watch its JSON Schema change.
- Send invalid arguments and see validation reject them.
- Pass the check.

## The read

### A tool is a function with a schema

tool(fn, { name, description, schema }) from @langchain/core/tools turns an async function into a tool. The schema is a zod object: z.object({ order\_id: z.string().describe("The order number") }). It says which arguments the tool takes, their types, and which ones are optional.

The schema serves twice. LangChain converts it to JSON Schema, which is what the model reads. And your code uses it to check the arguments the model sends before your function runs.

### What the model sees

model.bindTools(tools) returns a model that sends each tool's name, description and JSON Schema with every call. The function itself never leaves your code. The model only knows what these three tell it, so descriptions are prompts: write them for the model, and describe each field with .describe().

### A tool call and its answer

When the model wants a tool, its reply is an AIMessage with tool\_calls: each call has a name, args and an id. Your code runs the tool and answers with a ToolMessage that carries the same tool\_call\_id. A model can ask for several calls at once, so the id is what pairs each result with its call.

Pass the whole tool call to tool.invoke(). The tool validates the args against its schema, runs your function, and returns the ToolMessage for you. If the args do not match, it throws ToolInputParsingException and your function never runs. An agent loop catches the error and sends it back as a ToolMessage with status "error", so the model can fix its call. In chapter 6, ToolNode does this for you.

## Try it: From zod to tool call

Add a field to a tool's schema and see what the model reads. Then run the model's call through the real validation, once with valid arguments and once with arguments you choose.

https://agenticademy.org/en/paths/first-agents/05-tools-and-tool-calls

## Check

### Question 1 of 3

What does the model receive about a bound tool?

- The tool's source code.
- **✓** Its name, description and JSON Schema.
- Only its name.

**Why:** bindTools converts each tool to its provider's format: name, description and the input schema. The function itself never leaves your code.

### Question 2 of 3

What links a ToolMessage to the call it answers?

- The tool's name.
- **✓** The tool\_call\_id, equal to the id of the call in the AIMessage.
- Their position in the list.

**Why:** A model can ask for several calls at once, even of the same tool. The id pairs each result with its call.

### Question 3 of 3

The model sends order\_id as a number, but the schema says z.string(). What happens?

- The tool runs and converts it.
- **✓** Validation fails before your function runs, and the error can go back to the model.
- The model is retrained.

**Why:** The schema is enforced in your code. A loop can return the error as a ToolMessage so the model can try again.

## What you can do now

- Define a tool with tool() and a zod schema, with useful descriptions.
- Bind tools to a model and read its tool\_calls.
- Answer a tool call with a ToolMessage, and handle invalid arguments.

---

[Back to Build your first agents](README.md) · [Next: The agent loop: a minimal ReAct agent](06-the-agent-loop.md)
