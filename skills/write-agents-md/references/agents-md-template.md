# <Project Name> AGENTS.md

Brief one-liner describing what this project is and what this guide covers.

**Location:** `AGENTS.md` at the repository root.

## Project Structure Guide

### Overview

<1-2 sentences about the project.>

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

### Documentation Intent Principle

Documents record intent, code implements it. Intent is the source of truth.

- Write documents before changing code when intent changes. Name files by intent, not topic.
- Treat documents as living — record current intent, never intermediate states. Update when intent shifts; move obsolete docs to `docs/adr/superseded/`.

### Check Tiers

Checks are tiered by cost and environment requirements.

- **Fast tier** — lint, typecheck, unit tests. Deterministic, seconds to run, no special dependencies. Must stay runnable locally with one command.
- **CI tier** — e2e, visual (e.g. screenshot tests needing tmux + Chrome), performance, multi-environment. Keep out of the default local suite; run in CI.

- Put a new check in the fast tier unless it genuinely needs CI-only dependencies.
- When verifying changes, run the fast tier. Do not try to reproduce CI-tier checks locally.

### <Project-Specific Rule>

<Rule statement. Use imperative mood.>

<Condition: when this rule applies.>

<Exception: when it does not apply.>

## Operation Guide

### Development Workflow

1. <Step 1>:
   ```bash
   <command>
   ```
2. <Step 2>:
   ```bash
   <command>
   ```
3. <Step 3>

### Testing & Automated Checks

<When to run which checks.>

#### <Test category>

```bash
<command>
```

### Utilities & Tips

- <Tip>:
  ```bash
  <command>
  ```
