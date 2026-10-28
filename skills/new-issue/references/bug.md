# `bug` Example

**Title:** `fetch --dry-run` still overwrites existing config file

**Labels:** `bug`

```markdown
## What happened

Running `mycli fetch <url> --dry-run` overwrites the local `.mycli/config.json` file instead of only printing planned changes.

## What was expected

`--dry-run` should print the files that would be modified without writing to disk.

## Steps to reproduce

1. Create a `.mycli/config.json` with custom content.
2. Run `mycli fetch https://example.com/repo --dry-run`.
3. Inspect `.mycli/config.json` — the custom content is overwritten.
```
