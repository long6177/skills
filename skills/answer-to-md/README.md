# answer-to-md

[![skills.sh](https://skills.sh/b/long6177/skills)](https://skills.sh/long6177/skills)

**Turn an answer into a file, not a wall of chat.** Long, structured answers are often worth keeping — but they live in the scrollback and get lost. `answer-to-md` writes the assistant's complete answer to a UTF-8 Markdown file in the environment's output directory, and replies with just one line and a link.

## Why it exists

The answers actually worth keeping — a comparison table, a research note, a code walkthrough — are trapped in chat. Copying them out by hand loses the formatting; asking for a file usually means re-generating and losing fidelity. `answer-to-md` saves the real answer, as it was written, as a file.

## What you can do with it

- **Save a new answer** — `$answer-to-md <your question>`: complete the request, then save the final answer as Markdown.
- **Save the previous answer** — invoke `$answer-to-md` on its own: it saves the most recent substantive answer verbatim — wording, headings, links, citations, lists, tables, and code blocks preserved.
- **Get a clean file** — UTF-8 `.md`, a concise topic-derived filename with a `YYYYMMDD_HHmmss` timestamp to avoid overwrites, and a top-level heading added only if one is missing.
- **Keep chat short** — the reply is a single completion line plus a clickable link; the content lives in the file.

## Where it writes

It follows the environment, in this order: a designated user-facing directory (`outputs` / `artifacts` / `deliverables`), then `<task-or-session-root>/outputs/`, then `outputs/` under the working directory — never a temp or scratch location.

## Boundaries

- Passively invoked only — it never triggers on an ordinary request.
- It does not summarize, expand, or "update" a prior answer.
- If no verbatim prior answer is available, it asks instead of reconstructing or inventing one.

## Install

```bash
npx skills add long6177/skills --skill answer-to-md
```

Invoke it explicitly as `$answer-to-md`; it never auto-triggers.

## License

MIT
