---
name: write-readme-agents
description: Write or update README.md and AGENTS.md for a project. README.md is for users (human or agent) — what the project is and how to use it. AGENTS.md is for developer agents — where things are and how to build/test/modify. Use when creating these files from scratch, updating them with new rules or structure, or when asked to "maintain the repo guides".
---

# Write README & AGENTS.md

Use this skill when creating or updating `README.md` and `AGENTS.md`.

## README.md

README.md is for **users** — anyone who wants to understand and use the project. Keep it approachable, not strict.

### Mandatory Structure

Only three things are required:

#### 1. Opening paragraph (after H1 title)

The first paragraph after the top-level heading must explain **what the project is and what it does**.

#### 2. H2 — Installation

How to install and set up the project.

#### 3. H2 — Usage

How to use the project. Include runnable examples where helpful.

### Optional Sections

Everything else is optional: features, roadmap, goals, non-goals, FAQ, etc. No rules on these — add what's useful.

### Style

- Write for humans, but keep technical accuracy.
- Commands should be copy-paste-able.

## AGENTS.md

AGENTS.md is for **developer agents** — agents that modify the project. Follow the [OpenAI Agents Python convention](https://raw.githubusercontent.com/openai/openai-agents-python/main/AGENTS.md).

### Mandatory Three-Layer Structure

Every `AGENTS.md` must use `# AGENTS.md` as the single H1 title, followed by exactly these three H2 sections in this order:

| Section | Purpose | Must Answer |
|---------|---------|-------------|
| **Policies & Mandatory Rules** | Hard rules and skill triggers that constrain agent behavior | "What must the agent always / never do?" |
| **Project Structure Guide** | Directory layout, key files, architecture guidelines | "Where is everything and how is it organized?" |
| **Operation Guide** | Dev environment, workflows, test commands, tooling | "How do I run, test, and build this project?" |

If a section has no content yet, keep the H2 heading and add a placeholder — never omit a section. Any subsections inside these three required sections must start at H3.

### Key Style Principles

#### 1. Trigger-Based Rules

Every rule states exactly **when** it applies and **when** it does not:

```
Run `$skill-name` when you change:
- src/foo/ (runtime code)
- tests/ (test code)

Skip for docs-only changes, unless [condition].
```

Never write a rule without a trigger condition. "Always do X" is acceptable only when it truly has no exceptions.

#### 2. Skill-Centric

Encapsulate repeatable workflows as named skills with a `$` prefix: `$run-tests`, `$validate-schema`. Define each skill's trigger and skip conditions in the Policies section.

#### 3. Copy-Paste-able Commands

Every shell command must be complete and runnable:

```bash
make tests
```

Not: "run the tests". Never use ellipsis in command blocks. Use `<placeholder>` for user-specific values.

#### 4. Imperative Mood

All rules use imperative: "Run X", "Use Y", "Do not Z". No "You should", "We recommend".

#### 5. Specific File Paths

Reference actual paths: `src/agents/run.py`, not "the runner module".

## README ↔ AGENTS.md Boundary

| Audience | README.md | AGENTS.md |
|----------|-----------|-----------|
| What the project is and does | ✅ | — |
| Installation and usage | ✅ | — |
| Feature list, roadmap | ✅ | — |
| Directory structure, key files | — | ✅ |
| Build / test / format commands | — | ✅ |
| Architecture guidelines | — | ✅ |
| Mandatory rules and skill triggers | — | ✅ |

**Rule**: if content serves **users**, put it in README.md. If it serves **developer agents modifying the project**, put it in AGENTS.md. Never duplicate between the two — pick one.

## How to Update

### When to Update

- README.md: project purpose, install steps, or usage changes.
- AGENTS.md: new rules, new skills, directory restructuring, build/test command changes, architecture guideline changes.

### Update Workflow

1. Read the existing file first.
2. For AGENTS.md, identify which layer the content belongs to (Policies, Structure, Operation).
3. Add content under the correct section.
4. Update the Table of Contents if headings changed.
5. Keep each section ordered by importance — most critical first.

## Templates

- `references/readme-template.md` — README.md skeleton.
- `references/agents-md-template.md` — AGENTS.md skeleton.
