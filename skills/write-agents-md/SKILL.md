---
name: write-agents-md
description: Write or update AGENTS.md for a project. AGENTS.md is for developer agents — where things are and how to build/test/modify.
---

# Write AGENTS.md

Use this skill when creating or updating `AGENTS.md`.

## Mandatory Structure

Every `AGENTS.md` must use `# AGENTS.md` as the single H1 title, followed by these H2 sections in this order:

| Section | Purpose | Must Answer |
|---------|---------|-------------|
| **Project Structure Guide** | Directory layout, key files, architecture guidelines | "Where is everything and how is it organized?" |
| **Domain Language** | Project-specific terminology agents cannot infer from code | "What do these words mean in this project?" |
| **Policies & Mandatory Rules** | Hard rules that constrain agent behavior | "What must the agent always / never do?" |
| **Operation Guide** | Dev environment, workflows, test commands, tooling | "How do I run, test, and build this project?" |

If a section has no content yet, keep the H2 heading and add a placeholder — never omit a section. Any subsections must start at H3.

**Project Structure Guide** must include a `### Repo Structure & Important Files` subsection with a directory tree (see Principle #5). **Domain Language** formatting rules are detailed in Principle #6.

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
│       └── archive/            # Superseded decisions
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

The following subsections are recommended additions under `Policies & Mandatory Rules` in every AGENTS.md. Full templates and examples are in `references/agents-md-template.md`.

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

1. Read the existing `AGENTS.md` first.
2. Identify which section the content belongs to (Structure, Domain Language, Policies, Operation).
3. Add content under the correct section.

## Templates

- `references/agents-md-template.md` — AGENTS.md skeleton.
