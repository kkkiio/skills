---
name: setup-speculative-flow
description: Set up speculative execution workflows (autonomous PR review loop and human-gated draft release) in GitHub Actions, PR templates, and AGENTS.md.
disable-model-invocation: true
---

# Setup Speculative Flow

Configure speculative execution workflows in the repository and document them in `AGENTS.md`.

In a speculative flow, the agent executes, verifies, and self-heals on an isolated side path (a PR branch or a GitHub Release Draft) behind a gate — reducing human involvement to a single decision point:

- **Spec Gate → Autonomous PR Merge** (`pr-review`): Once the human aligns on the design/spec and asks to open a PR, the agent implements it on a branch, writes a high-signal PR report following `.github/PULL_REQUEST_TEMPLATE.md` (goal, unknown unknowns & plan drifts, visual explanation / live evidence), self-heals CI and Codex review findings until the current `HEAD` receives an LGTM (`require-lgtm: true`), and squash-merges automatically.
- **Autonomous Draft → Human Publish Gate** (`draft-release`): When the human asks to release a new version, the agent bumps versions, verifies, tags, and prepares a GitHub Release Draft, stopping so the human can ship by clicking **Publish**.

## Available Flows

| Flow | Gate | Purpose | Apply When |
| ---- | ---- | ------- | ---------- |
| [`pr-review`](references/pr-review.md) | **AI + CI Gate** (`Codex Review Check` with `require-lgtm: true` → auto-merge) | Configures `.github/workflows/codex-review-check.yml`, `.github/PULL_REQUEST_TEMPLATE.md`, and `AGENTS.md` so the agent opens a high-signal PR, self-heals up to 3 rounds until LGTM, and squash-merges | **Default** (all GitHub repositories) |
| [`draft-release`](references/draft-release.md) | **Human Publish Gate** (Draft Release → `release.yml`) | When asked to release a new version, agent prepares a verified tag and Draft Release; maintainer clicks **Publish** to trigger CI publishing | Projects that publish packages or release artifacts |

## Workflow

Check whether target flows are specified below or in the user message:

1. **Specified** (e.g. `pr-review`, `draft-release`, `all`, or `default`):
   - If `default`, select `pr-review` plus `draft-release` if the project publishes packages/artifacts.
   - Read the matching file(s) from `references/` and configure the `.github/` files and `AGENTS.md` sections immediately.
2. **Not specified** (invoked with no extra input):
   - Inspect the repository (existing `.github/workflows/`, `.github/PULL_REQUEST_TEMPLATE.md`, package manifests, and `AGENTS.md`).
   - Recommend applicable flows from the table above (noting which are already configured).
   - Ask the user which flows to set up, then read the selected `references/` files and apply them.
