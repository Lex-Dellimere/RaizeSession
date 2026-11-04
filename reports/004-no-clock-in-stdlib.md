# 004: No wall clock in the stdlib; every caller supplies `now`

**Kind:** stdlib missing feature (ecosystem report
`../../reports/018-stdlib-no-time-api.md`). **Status:** open, designed around.

Mettle's stdlib has no wall-clock API outside `std/win32`. This library
therefore never reads a clock: every time-dependent function takes
`now: int64` (unix seconds). That is also what makes the expiry tests
deterministic, so it would stay even with a clock, but every application
has to obtain unix time itself (a hand-declared `clock_gettime` extern, or
its framework's helper such as raize's `rz_time_epoch`).

**What would help:** `std/time` with a wall clock in seconds; the README
could then show a one-line `now` in the quick start.
