# 001: Everything that calls crypto is main-thread only

**Kind:** Mettle interop limit, inherited from crypto. **Status:** open, documented.

Every random byte, SHA-256, HMAC-SHA256, base64url conversion,
constant-time comparison and wipe in this library is a call into the crypto
library, which is libsodium through Mettle's C interop. crypto's
reports/001 (and the ecosystem report `../../reports/032-foreign-libraries-main-thread-only.md`)
explain why that is main-thread only: threads Mettle starts install their
own thread pointer, which libc's `errno` and libsodium's lock do not
recognize, so a foreign call from such a thread touches memory that is not
libc's thread block.

**Effect here:** `ses_init`, `ses_close`, `ses_create`, `ses_lookup`,
`ses_rotate`, `ses_revoke_token`, `ses_csrf_token`, `ses_csrf_verify`,
`ses_sign`, `ses_verify` and `ses_token_*` are documented `thread: main
thread only`. Functions that reach only the store (`ses_save`,
`ses_touch`, `ses_revoke`, `ses_revoke_subject`, `ses_prune`) are also
main-thread only, because the store contract says the library calls the
store from the main thread. The data map, flash, cookie and config-check
functions are pure Mettle and may run on any thread.

For a server this means session work (lookup on each request) happens on
the main thread. A framework whose handlers run on worker threads (raize
handlers do) has to look the session up on the main thread before handing
the request to a worker, or marshal session calls back to it.

**What would fix it:** Mettle threads that are also libc/pthread threads
when a program binds a shared library; then this library could say "any
thread, one call at a time per SesManager".

**Not verified:** what happens when a Mettle-started thread calls these
functions anyway; no test does it, because the rule forbids it.
