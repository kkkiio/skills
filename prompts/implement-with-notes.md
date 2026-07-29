---
description: Implement changes while keeping drift notes to log deviations and edge cases
---
Implement the requested changes. While working, keep a drift notes file at `.agents/drift-notes/<feature>.md` to log any deviations, edge cases, or conservative choices you make.

Follow these rules for drift notes:

- Start the file with `# <feature> — drift notes` and `Temporary. Delete when done.`
- One bullet per deviation (short), or a short paragraph when the problem needs explanation.
- Delete the file once the deviations are resolved or incorporated into permanent docs.

Example format:

```markdown
# File upload — drift notes

Temporary. Delete when done.

## Deviations

- **S3 streaming**: Planned to stream directly to S3, but the reverse proxy
  enforces a 10MB body limit → buffer to temp dir, upload in chunks.

- **Session invalidation**: Redis key-space notifications aren't available
  on the shared cluster. Polling every 60s with a grace window is simpler and
  avoids the operational risk of a pub/sub dependency for an infrequent event.
```
