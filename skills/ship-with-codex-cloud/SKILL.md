---
name: ship-with-codex-cloud
description: Offload coding tasks to Codex Cloud — a remote sandbox that writes code, runs tests, and opens a PR. Use when the task needs an isolated environment or when you want a PR-ready branch without local execution.
---

# Ship with Codex Cloud

Spawn a Codex Cloud agent for remote coding and PR creation:

```bash
codex cloud exec --env <ENV_ID> \
  "<specific, actionable task>"
```

`<ENV_ID>` references a cloud environment created in [Codex settings](https://chatgpt.com/codex/settings/environments). The environment defines the container setup (dependencies, tools, env vars).

If `codex cloud list` returns no environments, tell the user to create one in settings first — cloud environments aren't created from the CLI.

Use `codex cloud list` to browse past tasks, `codex cloud status` to check progress, and `codex cloud diff` / `codex cloud apply` to pull changes locally.

## How to Use

**Give intent and context, not implementation details.** Same as `ship-with-pi`: project documentation contains the architecture and design. Tell the agent what to achieve — it will figure out how.

- ✅ "Implement the rate limiter described in `docs/rate-limiting.md`. Open a PR when done."
- ✅ "Fix the bug where sessions expire prematurely — @ `docs/session-design.md`."
- ❌ Over-specifying files, function signatures, or line numbers.

**The agent produces the deliverable and opens a PR.** No need to prescribe the workflow.

## Before You Ship

**Verify feasibility first.** If the task is ambiguous, contradictory, or missing critical context, confirm with the user before spawning.

**The remote agent cannot ask questions.** It will produce something even if the task is impossible. Only ship tasks where the path from intent to result is clear.

## vs `ship-with-pi`

| | ship-with-pi | ship-with-codex-cloud |
|---|---|---|
| Environment | Local pi sub-agent | Remote Codex Cloud sandbox |
| Output | Local changes | PR-ready branch + PR |
| Use when | Quick local iteration | Isolated env, PR workflow |
