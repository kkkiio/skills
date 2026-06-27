# AGENTS.md

Agent guide for the `kkkiio/skills` collection — reusable skills for coding agents.

**Location:** `AGENTS.md` at the repository root.

## Table of Contents

1. [Policies & Mandatory Rules](#policies--mandatory-rules)
2. [Project Structure Guide](#project-structure-guide)
3. [Operation Guide](#operation-guide)

## Policies & Mandatory Rules

### Mandatory Skill Usage

#### `$write-readme-md`

Run `$write-readme-md` when adding a new skill that needs a README, or when the project description, install steps, or usage instructions change.

#### `$write-agents-md`

Run `$write-agents-md` when adding new mandatory rules, restructuring directories, or changing build/test workflows.

### Post-Push Verification

After every `git push` to `main`, run the install verification:

```bash
npx skills add kkkiio/skills -y -g
```

This validates that the repository is correctly structured and installable via the skills CLI. The `-g` flag ensures skills install to `~/.agents/skills/` — do not use any other target directory.

If installation fails, fix the issue before considering the push complete.

### Skill File Naming

- Every skill lives under `skills/<skill-name>/SKILL.md`.
- The filename `SKILL.md` is mandatory (uppercase).
- Optional templates and references live in `skills/<skill-name>/references/`.

## Project Structure Guide

### Overview

This repository is a collection of skills for the `npx skills` ecosystem. Each skill is a self-contained directory with a `SKILL.md` that defines instructions for coding agents.

### Repo Structure

```
skills/
├── write-agents-md/
│   ├── SKILL.md
│   └── references/
│       └── agents-md-template.md
└── write-readme-md/
    ├── SKILL.md
    └── references/
        └── readme-template.md
```

- `skills/` — all skills. Each subdirectory is one installable skill.
- `skills/<name>/SKILL.md` — the skill definition (frontmatter + instructions).
- `skills/<name>/references/` — optional supporting files referenced by the skill.
- `README.md` — user-facing project description and usage.
- `AGENTS.md` — this file.

### Skill Structure

Every `SKILL.md` has:

1. **YAML frontmatter** with `name` and `description`.
2. **H1 heading** matching the skill name.
3. **Mandatory structure** sections (per the skill's own rules).
4. **References** directory for templates and supporting files.

## Operation Guide

### Prerequisites

- Node.js (for `npx skills`)
- Git

### Development Workflow

1. Edit skill files in `skills/<skill-name>/SKILL.md`.
2. When changing a skill's own rules, use that skill to validate consistency (e.g., use `$write-readme-md` when editing the write-readme-md skill).
3. Commit and push:
   ```bash
   git add skills/<skill-name>/
   git commit -m "<type>(<skill>): <description>"
   git push
   ```
4. Verify the push by installing from the remote:
   ```bash
   npx skills add kkkiio/skills -y -g
   ```
5. Confirm skills land in `~/.agents/skills/`:
   ```bash
   ls ~/.agents/skills/<skill-name>/SKILL.md
   ```

### From Scratch: Creating a New Skill

1. Create the directory and SKILL.md:
   ```bash
   mkdir -p skills/<new-skill>/references
   ```
2. Copy an existing `SKILL.md` as a starting point and adapt.
3. Write the skill instructions.
4. Push and verify with `npx skills add kkkiio/skills -y -g`.
