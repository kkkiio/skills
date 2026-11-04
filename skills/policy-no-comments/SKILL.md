---
name: policy-no-comments
description: Add or update the "No Implementation Comments" policy in AGENTS.md.
disable-model-invocation: true
---

# Policy: No Implementation Comments

Add or update the following policy in `AGENTS.md` (replacing any existing comment policy section).
Adapt terminology to the project's language if needed (e.g. panic/Result instead of exceptions for Rust/Go).

### No Implementation Comments

When writing or modifying code, do not write comments that explain implementation details inside function bodies or internal logic.

Write API doc comments on public interfaces from a user-facing perspective, covering:

1. One-line summary.
2. Important parameters and return value.
3. Exceptions or errors that may be raised.
4. Non-obvious edge cases.
