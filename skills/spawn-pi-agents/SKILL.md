---
description: Spawn pi sub-agents via bash for exploration, code review, or parallel analysis. Use when the user asks to explore a codebase, audit code, or split work across multiple agents.
---

# Spawn Pi Agents

You can spawn sub-agent pi instances by running `pi` via bash. Use `--no-session` for one-shot tasks.

## Model & Thinking Selection

| Task Type | Model | Thinking | Why |
|-----------|-------|----------|-----|
| **Explore / Search / Read-only** | `deepseek/deepseek-v4-flash` | `high` | Fast, cheap, good enough for grep+read |
| **Code Review / Audit / Refactor / Implement** | `openai/gpt-5.5-codex` | `xhigh` | Strong at logic bugs, execution, landing changes |

If the user explicitly specifies a model or thinking level, use that instead.

## How to Spawn

### Explore (read-only)

```bash
pi -p --no-session \
  --provider deepseek --model deepseek-v4-flash --thinking high \
  --tools read,grep,find,ls \
  "<specific, focused prompt>"
```

### Code Review / Complex

```bash
pi -p --no-session \
  --provider openai --model gpt-5.5-codex --thinking xhigh \
  "<specific, focused prompt>"
```

## Guidelines

- Always use `--no-session` for one-shot sub-agents.
- Keep prompts focused and specific. Don't ask "explore the whole project", ask "find all files that handle X and report their structure".
- Summarize sub-agent output for the user; don't dump raw text.
- For explore tasks, the sub-agent should report file paths, key findings, and any issues — not read entire files unless needed.
