---
name: write-agents-md
description: Write or update AGENTS.md for a project. AGENTS.md is for developer agents — where things are and how to build/test/modify.
---

# Write AGENTS.md

Use this skill when creating or updating `AGENTS.md`.

## Scope and Inheritance

Treat the root `AGENTS.md` as the repository-wide guide. When working in a subtree, apply the root file and every `AGENTS.md` between the root and the target file.

Name each document in its H1 so multiple `AGENTS.md` files are distinguishable:

- Use `# <project-name> AGENTS.md` for the root file.
- Use `# <directory-name> AGENTS.md` for a nested file.

Write a nested `AGENTS.md` only as a delta from its ancestors:

- Add rules, terminology, paths, and commands that are specific to that subtree.
- Do not repeat inherited content.
- State an override explicitly and scope it to the subtree. The closest nested instruction takes precedence when it conflicts with an ancestor.
- Use only the headings needed for the delta; do not add empty sections or placeholders.

## Root Mandatory Structure

After its project-specific H1 title, the root `AGENTS.md` must contain these H2 sections in this order:

| Section | Purpose | Must Answer |
|---------|---------|-------------|
| **Project Structure Guide** | Directory layout, key files, architecture guidelines | "Where is everything and how is it organized?" |
| **Domain Language** | Project-specific terminology agents cannot infer from code | "What do these words mean in this project?" |
| **Policies & Mandatory Rules** | Hard rules that constrain agent behavior | "What must the agent always / never do?" |
| **Operation Guide** | Dev environment, workflows, test commands, tooling | "How do I run, test, and build this project?" |

If a root section has no content yet, keep the H2 heading and add a placeholder — never omit a section. Any root subsections must start at H3.

The root **Project Structure Guide** must include a `### Repo Structure & Important Files` subsection with a directory tree (see Principle #5). **Domain Language** formatting rules are detailed in Principle #6.

## Key Style Principles

### 1. Trigger-Based Rules

Every rule states exactly **when** it applies and **when** it does not:

```
When you change:
- src/foo/ (runtime code)
- tests/ (test code)

Run `make test && make lint`. Skip for docs-only changes.
```

Never write a rule without a trigger condition. "Always do X" is acceptable only when it truly has no exceptions.

### 2. Copy-Paste-able Commands

Every shell command must be complete and runnable:

```bash
make tests
```

Not: "run the tests". Never use ellipsis in command blocks. Use `<placeholder>` for user-specific values.

### 3. Imperative Mood

All rules use imperative: "Run X", "Use Y", "Do not Z". No "You should", "We recommend".

### 4. Specific File Paths

Reference actual paths: `src/agents/run.py`, not "the runner module".

### 5. Repo Structure Tree

For `Project Structure Guide` → `Repo Structure & Important Files`, use a plain `text` code block with a box-drawing tree:

```text
.
├── AGENTS.md                   # Root — 全局规则 + 索引子 AGENTS.md
├── tests/                      # Automated tests
├── docs/                       # Project documentation
│   └── adr/                    # Architecture Decision Records — current decisions
│       ├── 0001-use-jwt-for-sessions.md
│       ├── 0002-add-rate-limiting.md
│       └── superseded/         # Superseded decisions
│           └── 0000-use-sessions-in-redis.md
├── packages/                   # Multi-root sub-projects
│   ├── frontend/
│   │   └── AGENTS.md           # Sub — 前端专属规则
│   └── backend/
│       └── AGENTS.md           # Sub — 后端专属规则
├── examples/                   # 独立示例项目
│   └── chat-demo/
│       └── AGENTS.md           # Sub — 示例项目规则
├── .agents/                    # Agent working files
│   └── drift-notes/            # Temporary implementation deviation logs
│       └── file-upload.md
└── package.json                # Scripts and dependencies
```

### 6. Domain Language Entries

Every project has terms whose meanings differ from common usage — an agent misunderstanding them causes wrong implementation. Good candidates for domain terms are easy to spot: they're the words users say repeatedly, and they're typically English terms that overlap with type names in code. The `Domain Language` section must have:

- One entry per line: `- **Term** — One-sentence definition of what it IS (not what it does).`
- `_Avoid_: synonym` only when an agent might substitute a confusing synonym.
- No relationships, history, implementation details, or maintenance rules.
- Keep under 20 lines — each line costs tokens in every session.

## Recommended Policies

The following subsections are recommended additions under `Policies & Mandatory Rules` in the root `AGENTS.md`. Add one to a nested `AGENTS.md` only when the subtree introduces or overrides that policy. Full templates and examples are in `references/agents-md-template.md`.

### Compatibility Policy

When project stability expectations affect implementation choices, add a `Compatibility Policy` subsection under `Policies & Mandatory Rules`.

For example, a self-use or 0.x project might state: zero stability guarantees, prefer direct migrations over compatibility layers.

### Documentation Intent Principle

Documents record intent, code implements it. Add a `Documentation Intent Principle` subsection under `Policies & Mandatory Rules`.

### Drift Notes

Implementation inevitably surfaces unknown unknowns that force deviations from the plan. Log them in temporary notes so the next session (human or agent) can pick up the context. Add a `Drift Notes` subsection under `Policies & Mandatory Rules`.

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
| Mandatory rules | — | ✅ |

If content serves **developer agents modifying the project**, put it in AGENTS.md. Never duplicate between README and AGENTS.

## When to Update

Update AGENTS.md when:
- New mandatory rules are established
- New domain terms are defined or existing ones change meaning
- Directory restructuring occurs
- Build/test commands change
- Architecture guidelines change

## Update Workflow

1. Locate the target file, then read the root `AGENTS.md` and every existing `AGENTS.md` on the path to it.
2. For the root file, identify which mandatory section the content belongs to (Structure, Domain Language, Policies, Operation).
3. For a nested file, remove content already supplied by an ancestor and keep only subtree-specific additions or explicit overrides.
4. Add the content under the appropriate heading.

## Templates

- `references/agents-md-template.md` — root AGENTS.md skeleton.
