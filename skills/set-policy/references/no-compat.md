### No Backward Compatibility

Do not maintain backward compatibility when changing APIs, persisted data, config files, CLI flags, or workflows:

- Do not add compatibility layers, deprecated aliases, dual-format parsers, or fallback shims.
- Migrate call sites and data formats directly to keep current-state code simple.
