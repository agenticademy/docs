# Tokens

Understanding models · Chapter 2 of 8 · 14 minutes

Models read and write text as tokens, each family of models splits text its own way, and both prices and context limits are counted in tokens.

## Goals

- Split one sentence with the tokenizers of six families of models.
- Switch to Spanish, Japanese and code, and compare the counts.
- Pass the check.

## The read

### Text becomes tokens

In the last chapter you met many kinds of models. The ones that work with text all start the same way. A tokenizer, a small program that comes with each model, cuts the text into tokens: pieces that are often a whole word with the space before it, and sometimes part of a word, a sign or a digit. Each token is then swapped for a number, its token id. The model reads only these numbers. When it writes, it gives back token ids, and the tokenizer turns them into text.

### Each family has its own vocabulary

The list of pieces a tokenizer knows is its vocabulary. It is built from a large sample of text before the model is trained: pieces that appear often there become one token each. Each family of models, such as GPT, Llama or Qwen, has its own tokenizer, and a new generation may get a new one. The six in the lab know between 32,768 and 262,144 pieces.

A word that is not in the vocabulary is built from smaller pieces. At the very end are bytes, the small codes a computer uses to store each character. A Japanese character or an emoji takes three or four bytes, so a tokenizer that lacks it needs several tokens for one character.

### Why the count matters

Providers price their models per million tokens: the tokens you send, called input, and the tokens the model writes, called output. A model's context window, the most text it can take in at once, is counted in tokens too. Text that needs more tokens costs more and fills the window sooner. In the lab, the same request takes more tokens in Spanish and Japanese than in English in every family, and code and emoji fit fewer characters into each token than English does.

### What is in the lab

Six tokenizers: OpenAI's for GPT-4o, a closed model, and those of five families with open weights, whose model files are published: Gemma 4 from Google, Llama 3 from Meta, Qwen3, DeepSeek-V3 and Mistral v3. Their splits were recorded with their real tokenizer files. The last step is optional: it downloads the files you pick, with their size shown first, and splits your own text in this page.

## Try it: Tokenizer lab

Split one sentence with six tokenizers, then switch the text and watch the counts change.

https://agenticademy.org/en/paths/understanding-models/02-tokens

## Check

This German word means “table reservation confirmation message”. GPT-4o makes 10 tokens of the lab's English sentence, about one per word. How many does it make of this one word?

The word:

```
Tischreservierungsbestätigungsnachricht

1 word, 39 letters
GPT-4o tokens: ?
```

- 1, because it is one word.
- **✓** 9, because it is built from longer pieces that are in the vocabulary.
- 39, one for each letter.
- None, because a model leaves out words it has never seen.

**Why:** GPT-4o has no single token for this rare word, so its tokenizer builds it from 9 pieces it knows: T, isch, reserv, ierungs, best, ät, igungs, nach, richt. It is not 1, because only common words get a token of their own. It is not 39, because runs of letters that appear often are stored as one piece. And nothing is left out: any text can be built from pieces, down to single bytes. Other families split it differently: Mistral v3 needs 13 tokens.

## What you can do now

- Explain what a tokenizer, a token and a vocabulary are.
- Say why the same text can be a different number of tokens in two families of models.
- Explain why prices and context windows depend on the token count, and why other languages, code and emoji often cost more.

---

[Back to Understanding models](README.md) · [Next: Choosing the next token](03-next-token.md)
