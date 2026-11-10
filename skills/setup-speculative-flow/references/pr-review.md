# Flow: Autonomous PR Review (`pr-review`)

Configure [kkkiio/codex-review-check](https://github.com/kkkiio/codex-review-check) with `require-lgtm: true` in the repository and document the speculative PR execution, high-signal PR description requirements, and auto-merge loop in `AGENTS.md`.

## Steps

1. **Install or update `.github/workflows/codex-review-check.yml`**:
   Fetch the latest example workflow from `kkkiio/codex-review-check` and ensure `require-lgtm: true` is set under `with:` so every merged `HEAD` is attested by a fresh Codex LGTM (not just resolved threads on an older commit):

   ```bash
   mkdir -p .github/workflows
   curl -fsSL https://raw.githubusercontent.com/kkkiio/codex-review-check/main/examples/codex-review-check.yml -o .github/workflows/codex-review-check.yml
   ```

   Ensure the step inputs in `.github/workflows/codex-review-check.yml` include `require-lgtm: true`:

   ```yaml
         - name: Check Codex review state
           uses: kkkiio/codex-review-check@<pinned-sha>
           with:
             github-token: ${{ github.token }}
             pull-request: ${{ inputs['pull-request'] || github.event.pull_request.number }}
             require-lgtm: true
   ```

2. **Add or update `### PR Workflow` in `AGENTS.md`**:
   Place the following section in `AGENTS.md` (under `## Operation Guide` if present, matching the document's language):

   ```markdown
   ### PR Workflow

   GitHub PRs are gated by [Codex Review Check](.github/workflows/codex-review-check.yml) (`require-lgtm: true`).

   When asked to open or update a PR (e.g. after aligning on a design/spec):

   1. **Write a high-signal PR description**: Treat the PR body as an implementation report for the human — do not restate file-by-file diffs or routine CI test logs. Include:
      - **Goal**: Why this change is being made and the user/system outcome.
      - **Unknown Unknowns & Plan Drifts**: Surprises discovered during implementation, edge cases not covered in the initial discussion, and any deviations or trade-offs from the agreed plan (state `None` if it matched the plan).
      - **Visual Explanation & Live Evidence**: Mermaid diagrams or before/after comparisons for non-trivial flows, plus real verification evidence beyond CI unit tests (UI screenshots for web/visual changes, live test runs, or eval results).
   2. **Self-heal CI & Codex Review in the background**:
      - Wait for checks with `gh pr checks --watch`.
      - If CI or `Codex Review Gate` fails, inspect the failure with `gh run view <run-id> --log-failed`. Fix valid findings (or reply with reasoning and resolve the thread), push, and trigger `@codex review` as instructed until the current `HEAD` receives an LGTM.
      - Update the PR description if fixes introduce new plan drifts or findings worth noting.
   3. **Auto-merge or escalate**:
      - Once all checks pass, squash-merge the PR (`gh pr merge --squash --delete-branch`).
      - Stop and discuss with the maintainer if the review loop does not pass after 3 rounds or a finding requires changing the agreed design/spec.
   ```

3. **Remind the user** (on first-time setup) to ensure Codex code review is enabled for the repository with the **On pull request** trigger.
