---
name: ship-with-pi
description: Offload coding and execution tasks — writing, editing, building, testing, installing dependencies, running scripts — to a pi sub-agent. Use when you need to get something done, not just understand it.
---

# Ship with Pi

Spawn a pi sub-agent for coding and execution:

```bash
pi -p --no-session \
  --provider openai --model gpt-5.6-terra --thinking high \
  "<specific, actionable task>"
```

## How to Use

**Give intent and context, not implementation details.** Project documentation already contains the architecture and design decisions. Tell the ship agent what to achieve and where to look — it will figure out how.

- ✅ "Implement the rate limiter described in `docs/rate-limiting.md`."
- ✅ "Fix the bug where sessions expire prematurely — @ `docs/session-design.md`."
- ❌ "In `src/foo/bar.ts`, add a private method `_checkToken` that takes a string, then modify line 42 to call it..." (over-specifying code paths)

**The ship agent should produce the deliverable autonomously.** Don't prescribe files, function signatures, or implementation steps unless they're non-obvious constraints.

## Before You Ship

**Verify feasibility first.** If you're unsure whether the task is achievable — unclear requirements, contradictory constraints, missing dependencies — don't spawn immediately. Confirm with the user.

**The sub-agent cannot ask questions.** It will produce something even if the task is impossible. That something will be wrong. Only ship tasks where the path from intent to result is clear.
