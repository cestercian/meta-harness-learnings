# 2026-11-06 — OpenClaw: outside fix closed, re-landed upstream (#158357 → #193137)

**What happened.** `openclaw/openclaw#158357` (voice-call: Twilio `<Say>` fallback greeting cut off, call hung up after ~5s) was *closed*, not merged, by `roboclaw-bot` on 2026-11-05 (~10:01Z). The fix landed instead via upstream `#193137` (commit `486a0bddbb95bb5d00ff1ddf47d6cbc00403400b`), which kept our approach — Say-fallback greeting plus silence-redirect instead of an early hangup — and added signed-webhook regression coverage.

**Learning.**
- OpenClaw has a second landing path besides ClawSweeper-verdict → `roboclaw-bot` merge: the bot can **close an outside PR and re-land the change in its own PR** with extra tests. A "closed" state is not a rejection; read the closing comment and check the referenced commit before counting it lost.
- For merge-count tracking, these don't count as our merges; treat them as accepted-design / credit-in-comment outcomes.
- Making a fix easy to re-land (small diff, clear repro, a regression test the bot can extend — here signed Twilio webhooks) is what survives. Real Twilio call timing remained unverified on both sides; webhook-level tests were the accepted bar.

**Action.** Delete the PR babysit on close-and-reland; don't reopen or argue; note the upstream commit in the harness file.
