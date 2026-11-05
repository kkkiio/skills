### Black-Box Testing

When writing or modifying tests, verify behavior only through public APIs or CLI entrypoints:

- Do not add `*_for_test` exports, expose private types or functions, or widen public facades for testability.
