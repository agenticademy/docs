# Agenticademy lessons

The text of every lesson on [agenticademy.org](https://agenticademy.org), a free academy for AI
and agents. Each chapter on the site has a short read, a lab you can play with and a check. This
repository holds the words: the read, the goals, what each lab asks you to do, the check with its
answer and what you can do afterwards. The labs themselves run on the site.

## What is here

Six learning paths in two tracks, 50 chapters, in six languages.

**Understand AI**: how AI and its models work, what agents are, and how to use them well. No code.

| Path | Level | Chapters |
|---|---|---|
| AI principles | Beginner | 8 |
| Understanding models | Beginner | 8 |
| Agents for everyone | Beginner | 8 |

**Build agents**: for developers, a first agent in LangGraph.js, then systems of agents, then long
and complex runs.

| Path | Level | Chapters |
|---|---|---|
| Build your first agents | Beginner | 8 |
| Agent systems and standards | Intermediate | 8 |
| Advanced agents | Advanced | 10 |

## Languages

| Language | Start here |
|---|---|
| English | [lessons/en](lessons/en/README.md) |
| Español | [lessons/es](lessons/es/README.md) |
| Français | [lessons/fr](lessons/fr/README.md) |
| Deutsch | [lessons/de](lessons/de/README.md) |
| Português | [lessons/pt](lessons/pt/README.md) |
| 日本語 | [lessons/ja](lessons/ja/README.md) |

## How it is organised

```
lessons/
  <language>/
    README.md              the tracks and paths
    <path>/
      README.md            what the path covers, what you need first, the chapters
      <chapter>.md         one chapter: goals, the read, the lab, the check, what you can do now
```

Each chapter ends with a link to the same chapter on the site, where its lab runs in your browser.

This text matches the site as published on 2026-11-09.

## Mistakes and suggestions

If you find an error, an unclear sentence or a translation that reads wrong, open an issue in this
repository. Say which language, path and chapter, quote the sentence, and say what you would
change.

## Credits and licence

The ideas in the AI principles path draw on *The AI Fluency Framework* by Rick Dakan and Joseph
Feller (with Anthropic), © 2025, CC BY-NC-SA 4.0. The path is written independently and is not
endorsed by the authors or Anthropic. Its last chapter carries the same credit.

The lesson text is released under the MIT License: see [LICENSE](LICENSE).
