---
name: setup-draft-release
description: Set up a human-gated GitHub Draft Release workflow and document the project's release procedure in AGENTS.md.
disable-model-invocation: true
---

# Setup Draft Release

Configure a two-phase release workflow gated by a GitHub Release Draft:

1. **Agent prepares the draft**: When asked to cut a release, the agent handles version bumping, pre-release verification or builds, tagging, and creating a **draft** GitHub Release with release notes — then stops without publishing.
2. **Human publishes → CI ships**: The maintainer reviews the draft on GitHub and clicks **Publish**, triggering `.github/workflows/release.yml` (`on: release: types: [published]`) to publish artifacts (npm, mooncakes, GHCR, etc.).

## Workflow

1. **Understand the project's release shape**:
   Inspect the repository (or ask the user if ambiguous) to determine:
   - Version source(s) (single manifest vs. multi-package workspace).
   - What the agent should do locally when preparing a draft vs. what GitHub Actions should publish when the release is published.
2. **Set up `.github/workflows/release.yml`**:
   - Trigger on `release: types: [published]`.
   - Use `cancel-in-progress: false` so an in-flight publish is not interrupted.
   - Verify the git tag matches the manifest version(s) before publishing, and keep publishing idempotent.
3. **Document `### Release` in `AGENTS.md`**:
   Record the project-specific steps for preparing a draft release and what `release.yml` publishes once the maintainer publishes the draft.
4. **Remind the user** of any required repository secrets or permissions.

## Examples

- [`references/mooncakes-example.md`](references/mooncakes-example.md) — Multi-module MoonBit workspace example (`release.yml` + `AGENTS.md` section).
