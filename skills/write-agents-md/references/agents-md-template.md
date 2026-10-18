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

### Compatibility Policy

When changing public APIs, persisted data, config files, CLI flags, plugin contracts, or user-facing workflows, follow this project's compatibility policy.

- Project at 0.x: zero stability guarantees.
- Prefer direct migrations and simple current-state code over compatibility layers.
- Document intentional breaking changes in the final response; update `CHANGELOG.md` when it exists or when the project already uses release notes.

### Agent Author

When the agent writes research docs under `docs/research/`, sign them with the agent's name, not the owner's.

### CI-Gated Checks

Checks are tiered by cost and environment requirements.

- **Fast tier** — lint, typecheck, unit tests. Deterministic, seconds to run, no special dependencies. Must stay runnable locally with one command.
- **PR CI tier** — e2e, visual (e.g. screenshot tests needing tmux + Chrome), performance, multi-environment. Run on PRs.
- **Tag CI tier** — image and artifact builds, deployment previews, massive regression tests. Run on tags.

When verifying changes, run the fast tier.

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
