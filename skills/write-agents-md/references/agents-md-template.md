# AGENTS.md Template

````markdown
# AGENTS.md

Brief one-liner describing what this project is and what this guide covers.

**Location:** `AGENTS.md` at the repository root.

## Table of Contents

1. [Policies & Mandatory Rules](#policies--mandatory-rules)
2. [Project Structure Guide](#project-structure-guide)
3. [Operation Guide](#operation-guide)

## Policies & Mandatory Rules

### Mandatory Skill Usage

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

### [Domain-Specific Rule]

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
├── docs/                   # [Product and architecture docs]
└── package.json            # [Scripts and dependencies]
```

### [Domain-Specific Architecture Guidelines]

[Guidelines for the core runtime/module/etc.]

- [Rule 1]
- [Rule 2]

## Operation Guide

### Prerequisites

- [Tool/version requirement]
- [Tool/version requirement]

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
