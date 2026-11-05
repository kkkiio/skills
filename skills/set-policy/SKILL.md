---
name: set-policy
description: Add or update project policies in AGENTS.md.
disable-model-invocation: true
---

# Set Policy

Add or update reusable policies in `AGENTS.md` (replacing any existing section for the same policy). Adapt terminology to the project's language when needed (e.g. `Result`/panic instead of exceptions for Rust/Go).

## Available Policies

| Policy | Purpose | Apply When |
| ------ | ------- | ---------- |
| [`no-comments`](references/no-comments.md) | Forbid implementation comments; require user-facing API docs | **Default** (all projects) |
| [`no-compat`](references/no-compat.md) | Forbid backward-compatibility layers, aliases, and fallback shims | **Default** (0.x or self-use projects) |
| [`black-box-test`](references/black-box-test.md) | Test only via public APIs/CLI; do not widen facades for tests | **Default** (all projects) |

## Workflow

Check whether target policies are specified below or in the user message:

1. **Specified** (e.g. policy names, keywords, or `default`):
   - If `default`, select all **Default** policies that fit the project.
   - Read the matching file(s) from `references/` and add or update them in `AGENTS.md` immediately.
2. **Not specified** (invoked with no extra input):
   - Inspect the repository and existing `AGENTS.md`.
   - Recommend **Default** policies plus any policies whose `Apply When` condition matches the project (noting which are already present).
   - Ask the user which policies to apply, then read the selected `references/` files and update `AGENTS.md`.
