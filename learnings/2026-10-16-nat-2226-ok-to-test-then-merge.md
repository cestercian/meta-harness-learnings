# 2026-10-16 — NAT `#2226` landed: `/ok to test <sha>` then `/merge`

Activity after the 2026-10-15 note (commit `0c674c0`). Core named harnesses (Prime, T3, Orca*, OpenClaw) were quiet: OpenClaw `#158348` / `#158357` still only carry `status: needs proof` (and `#158357` `merge-risk: availability`). One tracked NAT PR closed the loop on the contrib path we had been documenting.

## NeMo Agent Toolkit `#2226` merged (2026-10-15)

`willkill07` landed our streaming empty-retry fix (`fix(core): preserve streaming retry exceptions on empty replay`) the same afternoon IST as the prior note:

1. We had already fixed DCO (author email == `Signed-off-by`), yapf, and the Streaming caveat docs/test after `Somay-kousis` review.
2. Tip was a merge-from-`develop` commit (`2de22c4`).
3. Maintainer commented `/ok to test 2de22c4` (exact SHA), CI ran, then **APPROVED**, then commented `/merge`.

**Contrib posture (confirmed end-to-end):** external NAT PRs do not get CI until a maintainer posts `/ok to test <sha>` for the commit that will actually run. After a long-lived branch, every develop sync needs a fresh ok-to-test on the new tip. Landing is not a GitHub UI merge from the author side — the maintainer finishes with `/merge` after approve. Do not burn a maintainer round trip on formatting; run `pre-commit run --all-files` before each push.

Architecture of the fix itself is unchanged from the 2026-10-07 / 2026-10-13 notes: empty completion after a prior failure must re-raise `last_exception`; the guard cannot tell an exhausted one-shot iterator from a legitimately empty retry, so document that caveat and pin the 429-then-empty case.

## Elsewhere (no new learning)

- OpenClaw `#158348` / `#158357` — still proof-gated; no new maintainer or ClawSweeper design signal since the liveness/progress note.
- OrcaRouter-Lite `#166` — still open; maintainer tagged `@orcacode-review` earlier, no new review body.
- T3 / Prime / OrcaReplay / Orca-Code-Review — no new cestercian activity since the 2026-10-15 DEP0190 / OpenCode Beta note.

## Links

- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2226
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/issues/2223
