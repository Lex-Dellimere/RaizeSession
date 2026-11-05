# 002: No storage backend: applications must write a SesStore

**Kind:** missing feature, deliberately on hold. **Status:** open.

The library ships only the `SesStore` interface (src/store.mettle) and no
implementation: no in-memory table, no MeoSQL / flux_orm adapter, no alph
table. Internal storage is on hold until the alph actor runtime is
complete. `tests/teststore.mettle` is a test-only double and is not part of
the package.

**Effect:** every application writes five functions (put, get, del,
del_subject, prune) over its own database before it can use sessions. The
README has a skeleton.

**Shape kept ready for later:** only pointers, int64s and string views
cross the vtable; `SesRecord` is fixed-size with no pointers, so a SQL row
maps field by field (hash BLOB(32) primary key, subject indexed, expires
indexed), and a byte store can persist the raw struct plus
`SES_RECORD_VERSION`.

**What would close it:** a `session_meosql` (flux_orm) adapter package and,
once alph lands, an alph-table store; both outside this package so the
library keeps no storage dependency.
