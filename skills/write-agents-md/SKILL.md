---
name: write-agents-md
description: Write or update AGENTS.md for a project. AGENTS.md is for developer agents — where things are and how to build/test/modify. Use when creating AGENTS.md from scratch, adding new rules or skills, restructuring the repo, or when asked to "maintain the AGENTS guide".
---

# Write AGENTS.md

Use this skill when creating or updating `AGENTS.md`.

## Mandatory Structure

Every `AGENTS.md` must use `# AGENTS.md` as the single H1 title, followed by these H2 sections in this order:

| Section | Purpose | Must Answer |
|---------|---------|-------------|
| **Domain Language** | Project-specific terminology agents cannot infer from code | "What do these words mean in this project?" |
| **Policies & Mandatory Rules** | Hard rules and skill triggers that constrain agent behavior | "What must the agent always / never do?" |
| **Project Structure Guide** | Directory layout, key files, architecture guidelines | "Where is everything and how is it organized?" |
| **Operation Guide** | Dev environment, workflows, test commands, tooling | "How do I run, test, and build this project?" |

If a section has no content yet, keep the H2 heading and add a placeholder — never omit a section. Any subsections must start at H3.

## Key Style Principles

### 1. Trigger-Based Rules

Every rule states exactly **when** it applies and **when** it does not:

```
Run `$skill-name` when you change:
- src/foo/ (runtime code)
- tests/ (test code)

Skip for docs-only changes, unless [condition].
```

Never write a rule without a trigger condition. "Always do X" is acceptable only when it truly has no exceptions.

### 2. Skill-Centric

Encapsulate repeatable workflows as named skills with a `$` prefix: `$run-tests`, `$validate-schema`. Define each skill's trigger and skip conditions in the Policies section.

### 3. Copy-Paste-able Commands

Every shell command must be complete and runnable:

```bash
make tests
```

Not: "run the tests". Never use ellipsis in command blocks. Use `<placeholder>` for user-specific values.

### 4. Imperative Mood

All rules use imperative: "Run X", "Use Y", "Do not Z". No "You should", "We recommend".

### 5. Specific File Paths

Reference actual paths: `src/agents/run.py`, not "the runner module".

### 6. Repo Structure Tree

For `Project Structure Guide` → `Repo Structure & Important Files`, use a plain `text` code block with a box-drawing tree:

```text
.
├── src/                    # Runtime source
│   ├── core/               # Core domain logic
│   └── server/             # Server entrypoints
├── tests/                  # Automated tests
├── docs/                   # Project documentation
│   └── engineering/        # Current engineering intent — living docs
│       ├── authentication.md  # e.g. auth strategy, token lifecycle
│       ├── persistence.md     # e.g. storage decisions, data model constraints
│       └── ...
└── package.json            # Scripts and dependencies
```

### 7. Domain Language Entries

Every project has terms whose meanings differ from common usage — an agent misunderstanding them causes wrong implementation. Good candidates for domain terms are easy to spot: they're the words users say repeatedly, and they're typically English terms that overlap with type names in code. The `Domain Language` section must have:

- One entry per line: `- **Term** — One-sentence definition of what it IS (not what it does).`
- `_Avoid_: synonym` only when an agent might substitute a confusing synonym.
- No relationships, history, implementation details, or maintenance rules.
- Keep under 20 lines — each line costs tokens in every session.

## Recommended Policies

The following subsections are recommended additions under `Policies & Mandatory Rules` in every AGENTS.md.

### Compatibility Policy

When project stability expectations affect implementation choices, add a `Compatibility Policy` subsection under `Policies & Mandatory Rules`.

For self-use, early-stage, or 0.x projects, use a concise policy like:

```markdown
### Compatibility Policy

When changing public APIs, persisted data, config files, CLI flags, plugin contracts, or user-facing workflows, follow this project's compatibility policy.

- Project at 0.x: zero stability guarantees.
- Prefer direct migrations and simple current-state code over compatibility layers.
- Document intentional breaking changes in the final response; update `CHANGELOG.md` when it exists or when the project already uses release notes.
```

### Documentation Intent Principle

Documents record intent, code implements it. Add a `Documentation Intent Principle` subsection under `Policies & Mandatory Rules`:

```markdown
### Documentation Intent Principle

Documents record intent, code implements it. Intent is the source of truth.

- Write documents before changing code when intent changes.
- Treat documents as living — record current intent, never intermediate states. Update or delete when intent shifts.
- Git preserves document history; documents preserve current intent. Delete obsolete docs without hesitation.
```

## README vs AGENTS Boundary

| Audience | README.md | AGENTS.md |
|----------|-----------|-----------|
| What the project is and does | ✅ | — |
| Installation and usage | ✅ | — |
| Feature list, roadmap | ✅ | — |
| Domain terminology (project-specific) | — | ✅ |
| Directory structure, key files | — | ✅ |
| Build / test / format commands | — | ✅ |
| Architecture guidelines | — | ✅ |
| Mandatory rules and skill triggers | — | ✅ |

If content serves **developer agents modifying the project**, put it in AGENTS.md. Never duplicate between README and AGENTS.

## When to Update

Update AGENTS.md when:
- New mandatory rules are established
- New skills are added with trigger conditions
- New domain terms are defined or existing ones change meaning
- Directory restructuring occurs
- Build/test commands change
- Architecture guidelines change

## Update Workflow

1. Read the existing `AGENTS.md` first.
2. Identify which section the content belongs to (Domain Language, Policies, Structure, Operation).
3. Add content under the correct section.
4. Update the Table of Contents if headings changed.
5. Keep each section ordered by importance — most critical first.

## Templates

- `references/agents-md-template.md` — AGENTS.md skeleton.
