# 003: mbuild cannot declare test programs; suites live in a separate package

**Kind:** mbuild missing feature. **Status:** open, worked around.

Same gap as crypto's reports/004. The suites reach libsodium through
crypto, and `mbuild test` runs `@test` functions in the compiler's
interpreter, which cannot call foreign C, so they must be built programs.
mbuild has no way to declare test programs of a library package, and
`mb_bin`s in session's own `mbuild.mettle` would make them bins of the
library.

**Workaround:** `tests/` is its own package (`session_tests`, one `mb_bin`
per suite, `mb_dep_path("session", "..")`), plus `tests/dependent/`. The
root command `mbuild test-programs` builds both in debug and release via
`$MBUILD_EXE` and runs every binary. Every test build doubles as proof that
crypto's `mb_link_libs(":libsodium.so.23")` reaches a dependent of a
dependent (it does: `readelf -d` shows `NEEDED libsodium.so.23` and
`RUNPATH $ORIGIN` on the suites and on `session_dependent`).
