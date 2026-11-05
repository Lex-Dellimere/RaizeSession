# 005: Not in the MVP

**Kind:** missing items. **Status:** open.

- **Concurrent requests on one session.** Each request loads its own copy
  of the record and `ses_save` replaces the whole record: the last write
  wins, so two parallel requests that both change data can lose one change,
  and a flash queued by one can be overwritten by the other. A versioned
  put (compare-and-swap on a counter in `SesRecord`) would need a store
  contract change.
- **Atomic rotation.** `ses_rotate` is put(new) then del(old), with a
  compensating del(new) on failure; a store with transactions cannot be
  asked to wrap the pair, because the vtable has no rekey/transaction call.
- **Listing a subject's sessions** ("your devices"): the vtable has no
  list call; only delete-by-subject.
- **Masked CSRF tokens.** The CSRF token is the same string for the whole
  session (until rotation). Under compression of responses that reflect
  attacker input (BREACH), a per-response masked token would be safer.
- **Signed values with expiry or encryption.** `ses_sign` authenticates
  but neither encrypts nor expires; an application embeds a timestamp in
  the value itself. crypto's XChaCha20-Poly1305 could back an encrypted
  variant.
- **`Expires` attribute.** Cookies get `Max-Age` only (every current
  browser honours it); old clients that need `Expires` would need an HTTP
  date formatter, which the stdlib lacks (see 004).
- **Lenient cookie parsing.** `ses_cookie_find` rejects headers some
  clients send (`a=b;`, `a=b;c=d`); there is no lenient mode.
