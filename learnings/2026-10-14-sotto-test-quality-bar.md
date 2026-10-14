# 2026-10-14 — Sotto `#493` merged: what a maintainer checks in a tests-only PR

Activity after the 2026-10-13 note (commit `a47be26`). Quiet day on the core harnesses (OpenClaw `#158348`/`#158357` only picked up `status: needs proof` labels; no new reviews on T3, Prime Agent, Orca, NemoClaw, Hermes, Omnigent). One merge worth recording.

## Sotto `#493` (merged 2026-10-13 by `Maxerns`)

Tests-only PR (`test(tui): cover theme-save failures and a successful retry`, closes `#451`). The first review was CHANGES_REQUESTED only because the branch no longer compiled after `#516` (`StatusNotice`), even though GitHub showed it as mergeable. After the rebase to `notice.message` it was approved and merged with the test logic unchanged.

**How the maintainer verified it:** they ran the tests on the branch, then **mutated the production failure path to silently succeed and confirmed both new tests fail**. A regression test only counts once it fails against the bug it pins. This is the same idea as OpenClaw's negative-control proof, applied by the reviewer instead of asked of us.

**Test-design choices the review called out as correct (reuse these):**

- Temp config paths, never the real user config.
- Force a filesystem failure with a **regular file where the parent directory should be**, not with permission bits (chmod-based failures break under root or in CI containers and differ by OS).
- Assert on the **error prefix** we own (`Failed to persist theme preference`), not on OS-specific error text.
- **Byte-for-byte preservation** check that a malformed existing file is left untouched on failure.
- No assertions tied to status styling.
- Cover the **successful retry in the same session**, so the error status gets replaced, not just raised.

**Gotcha reconfirmed:** "mergeable" on GitHub does not mean "compiles on current `main`". Rebase and run the suite before asking for re-review whenever an API in the touched area changed after the branch was cut (here `#516`).

## Links

- https://github.com/getsotto/sotto/pull/493
- https://github.com/getsotto/sotto/issues/451
