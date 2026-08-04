---
name: herdr-subagents
description: Coordinate long-lived explorer, reviewer, separate-repository worker, and worktree worker subagents from Pi in Herdr. Use when a primary Pi agent needs visible, reusable delegated agents and must choose a delegation pattern according to risk. Subagents may use any agent kind supported by Herdr. Requires HERDR_ENV=1.
---

# herdr-subagents

Use this skill as a guide for choosing and coordinating subagents, not as a fixed workflow. Herdr provides the persistent terminal, agent identity, lifecycle state, and follow-up channel; the primary agent remains responsible for deciding what is worth delegating.

## Verify the Herdr Context

Before issuing any control command, verify that this agent is running inside a Herdr-managed pane:

```bash
test "${HERDR_ENV:-}" = 1
```

If the check fails, say that you are not running inside Herdr and stop. Do not inspect or control the focused Herdr session from outside Herdr.

When the check passes, the `herdr` binary in `PATH` talks to the current session. Use it to inspect neighboring work, create terminal layout, start agents and commands, read output, and wait for state changes.

Treat the installed CLI as the authority for current syntax and supported agent kinds:

```bash
herdr agent
herdr workspace
herdr worktree
```

Do not run bare `herdr` for discovery because it launches or attaches the TUI. Read identifiers from the JSON returned by creation commands instead of predicting them.

## Keep Agents Long-Lived

Prefer a named, long-lived agent over a fresh agent for every small prompt. Modern agent contexts are large, runtimes compact them automatically, and accumulated project context usually makes follow-up work better.

Start an agent once, then continue through its Herdr name:

```bash
created=$(herdr workspace create --cwd "$PWD" --label "agent: <role>" --no-focus)
workspace_id=$(printf '%s\n' "$created" | jq -r '.result.workspace.workspace_id')
pane_id=$(printf '%s\n' "$created" | jq -r '.result.root_pane.pane_id')

herdr agent start <name> --kind <kind> --pane "$pane_id" -- <agent-args...>
herdr agent prompt <name> "<specific task>" --wait
herdr agent read <name> --source recent-unwrapped --lines 120
```

Use any Herdr-supported agent kind that fits the responsibility. Native agent arguments belong after `--`. A dedicated workspace is useful when the agent owns a durable role or a separate repository, but Herdr tabs and panes are equally available when another topology fits better.

Send related follow-up work to the same agent:

```bash
herdr agent prompt <name> "<follow-up based on the previous result>" --wait
```

Issue independent `agent prompt ... --wait` calls concurrently when several agents can work in parallel. If an agent becomes `blocked`, inspect it before responding:

```bash
herdr agent get <name>
herdr agent read <name> --source recent-unwrapped --lines 120
```

If terminal history cannot expose a long completed response, ask the agent to write the complete result as Markdown in a temporary directory and return the path, then read that file directly.

Do not end an agent merely because its first prompt completed. Keep explorers and reviewers available for likely follow-ups. After a worker has fulfilled its responsibility and the primary agent has integrated or otherwise accepted the result, actively close the worker's workspace:

```bash
herdr workspace close "$workspace_id"
```

## Choose a Delegation Pattern by Risk

### Explorer Subagents: High Value, Low Risk

Use explorers to map an unfamiliar codebase, locate ownership and entry points, compare possible implementations, find relevant tests, or investigate independent hypotheses. Give each explorer a distinct question and ask it to report evidence and conclusions without modifying the project.

Prefer a lightweight model such as Luna. For a Pi explorer, remove the built-in editing tools while retaining the rest of Pi's normal capabilities:

```bash
herdr agent start explorer --kind pi --pane "$pane_id" -- \
  --provider openai-codex \
  --model gpt-5.6-luna \
  --exclude-tools edit,write
```

Reuse the same explorer for deeper questions in the area it has already investigated. Start multiple explorers when the questions are genuinely independent, such as architecture, tests, and historical alternatives.

### Reviewer Subagents: Low Risk, Lower Value

Use reviewers for an independent pass over local or in-progress changes, especially when specialized knowledge or immediate feedback matters. Ask them to identify actionable findings without modifying the project.

For a Pi reviewer, use the same `--exclude-tools edit,write` arguments shown for the explorer. This removes the direct editing tools without introducing a separate sandbox mechanism.

Recognize that modern projects often receive remote LLM review from a GitHub pull request. Prefer that established review surface when it already meets the need; a local reviewer subagent is most useful before the PR exists, when feedback must be immediate, or when a focused specialist review adds something different.

### Separate-Repository Workers: Lower-Risk Implementation

Use a separate-repository worker when one broader task spans repositories but each repository can have one responsible agent. Give the worker a Herdr workspace rooted at its repository and keep it responsible for that repository across implementation, tests, and follow-up fixes.

For example, while building a game in one repository, delegate an optimization in the Selene game engine to a long-lived worker whose workspace is rooted at the Selene repository. The primary agent coordinates the cross-repository contract while the worker concentrates on the engine.

Use Terra or a stronger model for worker subagents. For a Pi worker:

```bash
herdr agent start selene-worker --kind pi --pane "$pane_id" -- \
  --provider openai-codex \
  --model gpt-5.6-terra
```

Close the worker deliberately after its repository responsibility is complete and its result has been integrated or accepted.

### Worktree Workers: Medium Risk, Usually Not Recommended

Use worktree workers only when the primary agent can decompose implementation cleanly and later review and merge the results. Parallel writers increase the difficulty of assigning ownership, maintaining compatible assumptions, and integrating branches even though their filesystems are isolated.

Use Terra or a stronger model for every worktree worker. A GitHub issue and pull request can reduce coordination risk: put the task boundary and acceptance criteria in the issue, let one worker own the worktree and resulting PR, then review the PR as the integration surface.

Herdr can create the worktree and its workspace together:

```bash
created=$(herdr worktree create \
  --cwd "$PWD" \
  --branch "<branch>" \
  --label "worker: <responsibility>" \
  --no-focus)

workspace_id=$(printf '%s\n' "$created" | jq -r '.result.workspace.workspace_id')
pane_id=$(printf '%s\n' "$created" | jq -r '.result.root_pane.pane_id')
```

Keep one worker responsible for each worktree. After the change has been reviewed and integrated, close the worker workspace rather than leaving an implementation agent running without an owner.

## Integrate the Results

Treat subagent output as input to the primary agent's judgment:

- Synthesize explorer evidence instead of forwarding raw transcripts.
- Decide whether reviewer findings are valid in the full project context.
- Coordinate interfaces and sequencing across separate-repository workers.
- Review and merge worktree workers with the same care as external contributions.
- Continue with the same named agent when a result needs clarification, correction, or deeper work.
