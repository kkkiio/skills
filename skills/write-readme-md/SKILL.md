---
name: write-readme-md
description: Write or update README.md for a project. README.md is for users (human or agent) — what the project is and how to use it. Use when creating README.md from scratch, updating install or usage instructions, or when asked to "maintain the README".
---

# Write README.md

Use this skill when creating or updating `README.md`.

## Mandatory Structure

Only three things are required:

### 1. Opening paragraph (after H1 title)

The first paragraph after the top-level heading must explain **what the project is and what it does**.

### 2. H2 — Installation

How to install and set up the project.

### 3. H2 — Usage

How to use the project. Include runnable examples where helpful.

## Optional Sections

Everything else is optional: features, roadmap, goals, non-goals, FAQ, etc. No rules on these — add what's useful.

## Style

- Write for humans, but keep technical accuracy.
- Commands should be copy-paste-able.

## README vs AGENTS Boundary

| Audience | README.md | AGENTS.md |
|----------|-----------|-----------|
| What the project is and does | ✅ | — |
| Installation and usage | ✅ | — |
| Feature list, roadmap | ✅ | — |
| Directory structure, key files | — | ✅ |
| Build / test / format commands | — | ✅ |
| Architecture guidelines | — | ✅ |
| Mandatory rules and skill triggers | — | ✅ |

If content serves **users**, put it in README.md. Never duplicate between README and AGENTS.

## When to Update

Update README.md when:
- Project purpose or description changes
- Install steps change
- Usage instructions change
- New major features are added

## Templates

- `references/readme-template.md` — README.md skeleton.
