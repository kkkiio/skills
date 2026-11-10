# Flow: Human-Gated Draft Release (`draft-release`)

Configure a two-phase release workflow gated by a GitHub Release Draft:

1. **Agent prepares the draft**: When asked to release a new version (e.g. "release a new version" / "帮我发新版本"), the agent handles version bumping, pre-release verification, tagging, and creating a **draft** GitHub Release with release notes — then stops and shares the draft URL without publishing.
2. **Human publishes → CI ships**: The maintainer reviews the draft on GitHub and clicks **Publish**, triggering `.github/workflows/release.yml` (`on: release: types: [published]`) to publish artifacts (npm, mooncakes, crates.io, GHCR, etc.).

## Steps

1. **Understand the project's release shape**:
   Inspect the repository (or ask the user if ambiguous) to determine:
   - Version source(s) (single manifest vs. multi-package workspace).
   - What the agent must run locally when preparing a draft (tests, builds, version sync) vs. what GitHub Actions publishes when the draft release is published.
2. **Set up `.github/workflows/release.yml`**:
   - Trigger on `release: types: [published]`.
   - Use `cancel-in-progress: false` so an in-flight publish is not interrupted.
   - Verify the git tag matches the manifest version(s) before publishing, and keep publishing idempotent.
3. **Add or update `### Release` in `AGENTS.md`**:
   Place the project-specific release instructions in `AGENTS.md` (under `## Operation Guide` if present, matching the document's language). State explicitly that when asked to cut or release a new version, the agent must prepare a **draft** GitHub Release (`gh release create v<version> --draft`) and stop for the maintainer to publish, plus what `.github/workflows/release.yml` publishes once published.
4. **Remind the user** of any required repository secrets or permissions.

## Examples

- [`mooncakes-example.md`](mooncakes-example.md) — Multi-module MoonBit workspace example (`release.yml` + `AGENTS.md` section).

