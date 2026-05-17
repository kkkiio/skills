---
name: write-agents-md
description: Write or update AGENTS.md files following the OpenAI Agents Python convention. Use when creating a new AGENTS.md from scratch, updating an existing one with new rules or structure changes, or when the user asks to "maintain AGENTS.md", "update the agent guide", or "document conventions for agents".
---

# Write AGENTS.md

Use this skill when creating or updating `AGENTS.md` files that follow the [OpenAI Agents Python convention](https://raw.githubusercontent.com/openai/openai-agents-python/main/AGENTS.md).

## Mandatory Three-Layer Structure

Every `AGENTS.md` must have exactly these three top-level sections, in this order:

| Section | Purpose | Must Answer |
|---------|---------|-------------|
| **Policies & Mandatory Rules** | Hard rules and skill triggers that constrain agent behavior | "What must the agent always / never do?" |
| **Project Structure Guide** | Directory layout, key files, architecture guidelines | "Where is everything and how is it organized?" |
| **Operation Guide** | Dev environment, workflows, test commands, tooling | "How do I run, test, and build this project?" |

If a section has no content yet, keep the heading and add a placeholder line — never omit a section.

## Key Style Principles

### 1. Trigger-Based Rules

Every rule states exactly **when** it applies and **when** it does not. Use this pattern:

```
Run `$skill-name` when you change:
- src/foo/ (runtime code)
- tests/ (test code)

Skip for docs-only or meta changes, unless [condition].
```

Never write a rule without a trigger condition. "Always do X" is acceptable only when it truly has no exceptions.

### 2. Skill-Centric

Encapsulate repeatable workflows as named skills with a `$` prefix: `$run-tests`, `$validate-schema`. Define each skill's trigger and skip conditions in the Policies section.

### 3. Copy-Paste-able Command Blocks

Every shell command must be a complete, runnable block:

```bash
make tests
```

Not: "run the tests". Never use ellipsis or placeholder arguments inside command blocks. If a command needs user-specific values, use `<placeholder>` with angle brackets.

### 4. Imperative Mood

All rules and instructions use imperative mood: "Run X", "Use Y", "Do not Z". No "You should", "We recommend", or "It would be good to".

### 5. Specific File Paths

Reference actual file paths, not vague descriptions. Write `src/agents/run.py` not "the runner module".

## How to Update AGENTS.md

### When to Update

- New rule or policy is established
- New skill is created that agents must use
- Directory structure changes
- Build/test commands change
- Architecture guideline changes

### When NOT to Update

- Temporary workarounds
- One-off task instructions
- Information already documented in README.md (human-facing docs stay in README)

### Update Workflow

1. Read the existing `AGENTS.md` first.
2. Identify which layer the new content belongs to (Policies, Structure, or Operation).
3. Add or modify content under the correct section.
4. Update the Table of Contents if headings changed.
5. Keep each section ordered by importance — most critical rules first.

## Template

See `references/template.md` for the AGENTS.md skeleton to use when starting from scratch.
