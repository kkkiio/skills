# <Project Name> AGENTS.md

Brief one-liner describing what this project is and what this guide covers.

**Location:** `AGENTS.md` at the repository root.

## Project Structure Guide

<1 sentence about the project structure.>

### Repo Structure & Important Files

```text
.
├── src/                    # <Runtime source>
│   ├── core/               # <Core domain logic>
│   └── server/             # <Server entrypoints>
├── tests/                  # <Automated tests>
├── docs/                   # <Project documentation>
│   └── adr/                # <Architecture Decision Records (MADR format) — current decisions>
│       ├── 0001-use-jwt-for-sessions.md
│       ├── 0002-add-rate-limiting.md
│       └── superseded/     # <Superseded ADRs moved here>
│           └── 0000-use-sessions-in-redis.md
└── package.json            # <Scripts and dependencies>
```

### <Domain-Specific Architecture Guidelines>

<Guidelines for the core runtime/module/etc.>

- <Rule 1>
- <Rule 2>

## Domain Language

Each entry is one sentence defining what the term IS, not what it does:

```markdown
- **Term** — One-sentence definition of what it IS.
  _Avoid_: synonym (only when a real synonym problem exists)
```

Keep entries minimal: no relationships, no history, no implementation details. Under 20 lines total.

## Policies & Mandatory Rules

### <Project-Specific Rule>

<Rule statement. Use imperative mood.>

<Condition: when this rule applies.>

<Exception: when it does not apply.>

## Operation Guide

### Development Workflow

```bash
# Example dev loop
just check
just fmt
just test
just build
```

### Utilities & Tips

- <Tip>:
  ```bash
  <command>
  ```
