---
name: pi-explore
description: Explore, search, and understand a codebase with a read-only pi sub-agent. Use when you need to find, trace, or investigate code without modifying anything.
---

# Pi Explore

Spawn a read-only pi sub-agent for codebase exploration:

```bash
pi -p --no-session \
  --provider openai-codex --model 'gpt-5.6 luna' --thinking high \
  --tools read,grep,find,ls,bash \
  "<focused exploration prompt>"
```

Keep prompts specific and scoped to what can actually be found by reading code. Summarize results — file paths, key findings, relevant snippets — don't dump raw output.
