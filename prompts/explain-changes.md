---
description: Generate an HTML report explaining recent changes with context, intuition, and a quiz
---
Generate a self-contained HTML report explaining the changes in this session. The report should help me (or a reviewer) understand what happened and why.

Include these sections:

1. **Summary** — one paragraph on what changed and why.
2. **Context & Intuition** — the unknowns we started with, the plan, and the reasoning behind key decisions.
3. **What Was Done** — a walkthrough of the changes, organized by area (not by file). Link to drift notes or ADRs where relevant.
4. **Key Decisions & Tradeoffs** — deviations from the plan, edge cases encountered, and conservative choices made.
5. **Quiz** — 3–5 questions testing understanding of the changes. Include an answer key (hidden behind a `<details>` toggle).

Requirements:
- Single `.html` file, self-contained (inline CSS, no external dependencies).
- Lead with a demo GIF or screenshot if one exists.
- Keep it scannable — reviewers should get the gist in 30 seconds.
- Write to `.agents/explain-<feature>.html`.
