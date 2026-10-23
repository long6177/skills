# no-more-unknown

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Explain it once, in words I already know.** Ask what a term means and most assistants answer with three more terms you have never seen. `no-more-unknown` makes every explanation bottom out in the words you already know — and when it can't, tells you exactly which words it left undefined.

## Why it exists

You share the tools and the project, but not every specialty the assistant works in. It cannot tell which words you already know, so it reaches for the nearest technical term, you ask what that means, and the answer uses another one. The conversation spirals without ever touching ground.

The usual advice — "explain simply" — does not fix this, because "simple" is judged by the person doing the explaining. This skill replaces it with a check that can be run: trace every specialty term back to something the user already knows, or to a plain definition.

## What it does

- **A known set** — everyday words, the vocabulary of the tools and the project, words you used yourself, and words already explained in this conversation. A specialty term that carries the answer gets defined at first use; one that doesn't is left out.
- **Bottom out** — before sending, the assistant traces each sentence back to that set. A definition containing another undefined word is circular and gets rewritten, not reworded.
- **One level down** — when you say you still don't understand, it does not swap synonyms. It drops to a concrete instance, then one analogy, then a one-line definition, and restates the original sentence in plain words.
- **A black-box list** — explanations end with the words they used without defining, so you can point at the one that is still a blur. You cannot ask about a word you cannot name.
- **Plans you can judge** — when you ask about a proposal, it also states what was rejected, what is a guess, what can break, and what it needs from you. That is what lets a non-expert push back instead of accept or distrust the whole thing.
- **The real word, kept** — terms are introduced with a plain gloss and then used normally, so you learn them instead of being shielded from them.

## When it applies

It is a response mode, not a house style. It governs what the assistant says when you ask about a concept, a term, a plan, or a piece of code — not every progress update or summary. Once invoked, it stays on for the session until you turn it off. Code comments and commit messages follow their own conventions.

## Install

```bash
npx skills add long6177/skills --skill no-more-unknown
```

Invoke it explicitly as `/no-more-unknown` (or `$no-more-unknown`); it never auto-triggers. Once on, it stays on for the rest of the session until you turn it off.

## Boundaries

- Not a "dumb it down" mode: the real term is taught, not removed.
- Not an auto-triggering style rule — the first version is invoked by you.
- It governs speech to you, not the artifacts written for other readers.

## License

MIT
