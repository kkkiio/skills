# AGENTS.md

Agent guide for the `kkkiio/skills` collection — reusable skills for coding agents.

## Policies & Mandatory Rules

### Skill Scope

- **Universal skills** (`write-agents-md`, `write-readme-md`) work in any project with any coding agent.
- **Tech-stack skills** (`write-snapshot-test` — MoonBit only) are generic baselines meant to be adapted into a concrete project (e.g. via `prompts/adopt-guidance.md`). Keep them project-agnostic: state rules generically and put project-specific values (commands, paths, fixture names, custom primitives) in examples marked `例：`.

### Skill File Naming

- Every skill lives under `skills/<skill-name>/SKILL.md`.
- The filename `SKILL.md` is mandatory (uppercase).
- Optional templates and references live in `skills/<skill-name>/references/`.

## Project Structure Guide

### Overview

This repository is a collection of skills and prompts for use with `agconf fetch`. Each skill is a self-contained directory with a `SKILL.md` that defines instructions for coding agents.

### Repo Structure

```text
skills/
├── write-agents-md/
│   ├── SKILL.md
│   └── references/
│       └── agents-md-template.md
├── write-readme-md/
│   ├── SKILL.md
│   └── references/
│       └── readme-template.md
└── write-snapshot-test/     # MoonBit only
    └── SKILL.md
prompts/
├── adopt-guidance.md
├── explain-changes.md
├── huh.md
├── implement-with-notes.md
├── restate-intent.md
└── teach-me.md
```

- `skills/` — all skills. Each subdirectory is one installable skill.
- `skills/<name>/SKILL.md` — the skill definition (frontmatter + instructions).
- `skills/<name>/references/` — optional supporting files referenced by the skill.
- `prompts/` — standalone prompts, installed for an explicitly selected agent runtime.
- `README.md` — user-facing project description and usage.
- `AGENTS.md` — this file.

### Skill Structure

Every `SKILL.md` has:

1. **YAML frontmatter** with `name` and `description`.
2. **H1 heading** matching the skill name.
3. **Mandatory structure** sections (per the skill's own rules).
4. **References** directory for templates and supporting files.

## Domain Language

- **Skill** — A self-contained `skills/<skill-name>/` directory whose `SKILL.md` teaches a coding agent reusable knowledge or behavior.
- **Prompt** — A standalone Markdown instruction under `prompts/` that is installed for an explicitly selected agent runtime.
- **Recommended policy** — An optional predefined subsection (e.g. Compatibility Policy, Check Tiers) that the write-agents-md skill may add to an AGENTS.md's `Policies & Mandatory Rules`; templates live in `skills/write-agents-md/references/`.

## Operation Guide

### Prerequisites

- Git
- agconf (for installing and updating skills and prompts)

### Development Workflow

1. Edit skill files in `skills/<skill-name>/SKILL.md`.
2. When changing a skill's own rules, use that skill to validate consistency (e.g., use `$write-readme-md` when editing the write-readme-md skill).
3. Commit and push:
   ```bash
   git add skills/<skill-name>/
   git commit -m "<type>(<skill>): <description>"
   git push
   ```
4. Ask the user whether to update the installed skill:
   ```bash
   agconf fetch https://github.com/kkkiio/skills/blob/main/skills/<skill-name> -g
   ```

### Installing & Updating

`agconf fetch` overwrites existing installed content by default (`--conflict overwrite`), so the same commands both install and update. Use `--dry-run` to preview.

```bash
# From GitHub (after pushing)
# All skills
agconf fetch https://github.com/kkkiio/skills -g

# A single skill
agconf fetch https://github.com/kkkiio/skills/blob/main/skills/<skill-name> -g

# A single prompt (prompts need an explicit agent runtime)
agconf fetch https://github.com/kkkiio/skills/blob/main/prompts/<prompt-name>.md -g -a <agent>
```