# Reports

Limitations and missing pieces found while building session, in the Mettle
ecosystem (compiler, mbuild, stdlib, sibling libraries). Open unless marked.

| # | Title | Kind |
|---|---|---|
| [001](001-main-thread-only.md) | Everything that calls crypto is main-thread only | Mettle interop limit (inherited) |
| [002](002-no-storage-backend.md) | No storage backend: applications must write a SesStore | missing feature (on hold) |
| [003](003-no-test-program-declaration.md) | mbuild cannot declare test programs; suites live in a separate package | mbuild missing feature |
| [004](004-no-clock-in-stdlib.md) | No wall clock in the stdlib; every caller supplies `now` | stdlib missing feature |
| [005](005-not-in-mvp.md) | Not in the MVP: concurrent-request merging, per-subject listing, masked CSRF, expiring/encrypted signed values | missing items |
