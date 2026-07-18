# AGENTS.md Template

````markdown
# AGENTS.md

Brief one-liner describing what this project is and what this guide covers.

**Location:** `AGENTS.md` at the repository root.

## Table of Contents

1. [Domain Language](#domain-language)
2. [Policies & Mandatory Rules](#policies--mandatory-rules)
3. [Project Structure Guide](#project-structure-guide)
4. [Operation Guide](#operation-guide)

## Domain Language

Each entry is one sentence defining what the term IS, not what it does:

```markdown
- **Term** — One-sentence definition of what it IS.
  _Avoid_: synonym (only when a real synonym problem exists)
```

Keep entries minimal: no relationships, no history, no implementation details. Under 20 lines total.

## Policies & Mandatory Rules

### Mandatory Skill Usage

Skills are accumulated over time. If the project has none yet, keep only this heading and the placeholder below — do not fabricate skills.

#### `$skill-name`

[Describe when this skill MUST be used.]

Run it when you change:
- [file/directory pattern]
- [file/directory pattern]

You can skip `$skill-name` for [list exceptions], unless [explicit condition].

#### `$another-skill`

[Description and trigger conditions.]

### Compatibility Policy

When changing public APIs, persisted data, config files, CLI flags, plugin contracts, or user-facing workflows, follow this project's compatibility policy.

- Project at 0.x: zero stability guarantees.
- Prefer direct migrations and simple current-state code over compatibility layers.
- Document intentional breaking changes in the final response; update `CHANGELOG.md` when it exists or when the project already uses release notes.

### Documentation Intent Principle

Documents record intent, code implements it. Intent is the source of truth.

- Write documents before changing code when intent changes.
- Treat documents as living — record current intent, never intermediate states. Update or delete when intent shifts.
- Git preserves document history; documents preserve current intent. Delete obsolete docs without hesitation.

### [Project-Specific Rule]

[Rule statement. Use imperative mood.]

[Condition: when this rule applies.]

[Exception: when it does not apply.]

## Project Structure Guide

### Overview

[1-2 sentences about the project.]

### Repo Structure & Important Files

```text
.
├── src/                    # [Runtime source]
│   ├── core/               # [Core domain logic]
│   └── server/             # [Server entrypoints]
├── tests/                  # [Automated tests]
├── docs/                   # [Project documentation]
│   └── engineering/        # [Current engineering intent — living docs]
│       ├── authentication.md  # [e.g. auth strategy, token lifecycle]
│       ├── persistence.md     # [e.g. storage decisions, data model constraints]
│       └── ...
└── package.json            # [Scripts and dependencies]
```

### [Domain-Specific Architecture Guidelines]

[Guidelines for the core runtime/module/etc.]

- [Rule 1]
- [Rule 2]

## Operation Guide

### Development Workflow

1. [Step 1]:
   ```bash
   [command]
   ```
2. [Step 2]:
   ```bash
   [command]
   ```
3. [Step 3]

### Testing & Automated Checks

[When to run which checks.]

#### [Test category]

```bash
[command]
```

### Utilities & Tips

- [Tip]:
  ```bash
  [command]
  ```
````
