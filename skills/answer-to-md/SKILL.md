---
name: answer-to-md
description: Save the assistant's complete answer as a UTF-8 Markdown file in the current environment's user-facing output location. Use this skill only when the user explicitly invokes `$answer-to-md` or explicitly asks to use `answer-to-md`; never trigger it automatically for an ordinary request. When the invocation includes a new request, complete that request and save the resulting answer. When invoked alone, save the most recent substantive assistant answer from the visible conversation.
compatibility: Requires filesystem write access to an environment-designated output directory or a writable task or workspace directory.
---

# Answer to Markdown

Create a Markdown deliverable instead of placing the full answer in chat. Preserve the quality and completeness the answer would have had without this skill.

## Select the content

Classify the invocation after removing the explicit skill name from the user's message.

- If substantive user content remains, treat it as a new request. Complete the task normally, including any authorized tool or file work, and use the complete final answer as the Markdown content.
- If no substantive content remains, find the most recent substantive assistant answer in the visible conversation and use that content. Ignore short confirmation-only messages produced by an earlier invocation of this skill, such as a message that only says a file was created and links to it.
- For a prior answer, preserve its wording, Markdown structure, links, citations, lists, tables, and code blocks. Do not reinterpret, summarize, expand, or silently update it.
- If no prior substantive answer is available verbatim, including when only a compacted summary is visible, do not reconstruct or invent it. Ask the user to paste or identify the content they want saved, and wait before creating a file.

## Resolve the output directory

Follow higher-priority environment and user instructions. Choose the destination in this order:

1. Use an explicitly designated user-facing deliverables directory from the current system, developer, app, or task context. Common directory labels include `outputs`, `artifacts`, and `deliverables`.
2. Otherwise, if a task or session root is known, create and use `<task-or-session-root>/outputs/`.
3. Otherwise, create and use `outputs/` under the current working directory.

Do not hardcode an absolute path from a previous session. Do not place the final Markdown file in `work/`, `scratch/`, a system temporary directory, or another location intended only for intermediate files. If the selected directory is not writable and no permitted fallback is available, explain the constraint and ask the user where to save the file.

## Name and write the file

- Derive a concise filename from the new question's topic or, for a standalone invocation, from the prior answer's topic.
- Append a local timestamp in `YYYYMMDD_HHmmss` form to prevent accidental overwrites, for example `python异步编程_20260821_153012.md`.
- Remove filesystem-reserved characters and trailing spaces or periods. Keep the topic portion concise. If a collision still occurs, append `-2`, `-3`, and so on rather than overwriting.
- Write the file as UTF-8 Markdown with the `.md` extension.
- Keep an existing top-level heading. If the content has no top-level heading, add one appropriate `#` heading without otherwise changing the content.
- Do not add frontmatter, execution logs, hidden reasoning, or a transcript unless the user's request asks for them.

## Verify and respond

Before replying, verify that the file exists, is non-empty, has a `.md` extension, and is inside the selected output directory.

In chat, return only a concise completion sentence and a clickable link to the generated Markdown file. Use the absolute local path and the link syntax required by the current environment. Do not duplicate the Markdown answer in chat.
