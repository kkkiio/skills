# `enhancement` Example

**Title:** Support `--json` output for `status` command

**Labels:** `enhancement`

```markdown
## What we want

Add a `--json` flag to `mycli status` that prints the current workspace state as a JSON object to stdout.

## Context

Scripts currently parse human-readable table output with `awk`, which breaks whenever column widths or headers change.

## Prior art

- `gh pr status --json`
- `cargo metadata --format-version 1`
```
