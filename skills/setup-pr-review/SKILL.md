---
name: setup-pr-review
description: Set up Codex Review Check workflow and add PR review instructions to AGENTS.md.
disable-model-invocation: true
---

# Setup PR Review

Configure [kkkiio/codex-review-check](https://github.com/kkkiio/codex-review-check) in the repository and document the automated PR review loop in `AGENTS.md`.

## Steps

1. **Install or update the GitHub Actions workflow**:
   Fetch the latest example workflow directly from `kkkiio/codex-review-check`:

   ```bash
   mkdir -p .github/workflows
   curl -fsSL https://raw.githubusercontent.com/kkkiio/codex-review-check/main/examples/codex-review-check.yml -o .github/workflows/codex-review-check.yml
   ```

2. **Add or update `### PR Review` in `AGENTS.md`**:
   Place the following section in `AGENTS.md` (under `## Operation Guide` if present, matching the document's language):

   ```markdown
   ### PR Review

   GitHub PRs are gated by [Codex Review Check](.github/workflows/codex-review-check.yml).

   - After opening or updating a PR, wait for checks with `gh pr checks --watch`.
   - If the `Codex Review Gate` check fails, run `gh run view <run-id> --log-failed` and follow its instructions (fix valid findings or reply with reasoning and resolve the thread, then push and trigger `@codex review` as instructed).
   - Once checks pass, squash merge the PR (`gh pr merge --squash`).
   - If the review loop does not pass after 3 rounds, stop and discuss with the maintainer.
   ```

3. **Remind the user** (on first-time setup) to ensure Codex code review is enabled for the repository with the **On pull request** trigger.
