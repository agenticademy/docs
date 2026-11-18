# Size and memory

Understanding models · Chapter 5 of 8 · 15 minutes

The memory a model needs comes from its parameters and the bits that store each one, plus room for its context; a mixture of experts uses a few of its parameters for each token but keeps them all in memory.

## Goals

- Slide Llama-3.1-8B down to a 4-bit type and see its file shrink.
- Check which versions fit in 8, 16, 24 and 80 GB.
- Compare total and active parameters for three mixture-of-experts models.
- Pass the check.

## The read

### Parameters take memory

In chapter 1 you met parameters, the numbers a model learned, also called weights. A name often gives their count: Llama-3.1-8B has about 8 billion. To run, a model must hold all its weights in memory: the fast storage a computer works from, or the memory on a graphics card (a GPU, a chip that does many sums at once). Memory is counted in bytes, and a byte is 8 bits. So the weights need parameters × bits per weight ÷ 8 bytes. At 16 bits each, 8 billion weights take 16 GB, 16 billion bytes.

### Fewer bits, a little worse

Quantisation, from chapter 4, stores each weight with fewer bits. Fewer bits can hold fewer values, so each weight is rounded more coarsely: the file shrinks, and the answers get a little worse, more so at the fewest bits. The visual uses measurements from llama.cpp, a free program that runs models on your own computer. Its type names give the bits: Q4\_K\_M keeps most weights in 4 bits and some in more, plus a little extra for each block of weights: 4.89 bits on average. It gives sizes in GiB: 1 GiB is 1,073,741,824 bytes, about 7% more than 1 GB.

### Room to work

A model fits when its weights, plus some room, fit in the memory you have. On a laptop that memory is shared with everything else that runs. The room is for the context: while a model works, it keeps what it worked out for each token, and that store grows with every token, up to the context window from chapter 2.

### Many experts, a few at a time

A mixture-of-experts model splits most of its layers into experts, smaller groups of weights. For each token, a small part called the router picks a few experts, and only they work. Their parameters, plus the parts every token uses, are the active parameters; all of them together are the total. A token costs about as much work as in a model of the active size, but the router may pick any expert for the next token, so the total must be in memory.

### Closed and open

Closed models such as Claude, GPT, Gemini and Grok publish no weights, and their model pages give no parameter counts, so every parameter count in the visual comes from an open model's own page, as of 2026-11-03.

## Try it: Will it fit

Slide one model to fewer bits, try four memory sizes, then see how much of a large model works on each token.

https://agenticademy.org/en/paths/understanding-models/05-size-and-memory

## Check

This mixture-of-experts model is quantised to about 4.9 bits per weight. What is the smallest of these memories that holds its weights?

The model:

```
Model:      gpt-oss-120b, open weights
Total:      117 billion parameters
Active:     5.1 billion for each token
Counts:     model card, 2026-11-03
Quantised:  4.9 bits per weight

Memory:     8 GB · 16 GB · 24 GB · 80 GB
```

- 8 GB: only the 5.1 billion active parameters must be in memory, about 3.1 GB.
- 16 GB: the 5.1 billion active parameters at 16 bits each, about 10.2 GB.
- **✓** 80 GB: all 117 billion parameters must be in memory, about 72 GB.
- None of them: 117 billion × 4.9 bits is about 573 GB.

**Why:** The router may pick any expert for the next token, so all 117 billion parameters must be in memory, not only the 5.1 billion one token uses: 117 billion × 4.9 bits ÷ 8 bits in a byte ≈ 72 GB, which leaves about 8 GB of the 80 for the context. 8 GB and 16 GB would hold only the active part, at 4.9 or at 16 bits. 573 GB forgets to divide by 8, the bits in a byte.

## What you can do now

- Estimate the memory a model's weights need from its parameters and bits per weight.
- Say what quantisation saves and what it costs.
- Tell whether a model fits in a given memory, with room left for its context.
- Explain total and active parameters in a mixture of experts, and why the total sets the memory.

---

[Back to Understanding models](README.md) · [Next: Open and closed models](06-open-and-closed.md)
