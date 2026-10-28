# 2026-10-28 — OpenClaw `#158348`: stale label survived a new head pushed by `cursor[bot]`

## What happened

- Barnacle v2 marked `#158348` stale on 2026-10-27 03:55Z.
- A rebased head was force-pushed on 2026-10-27 07:14Z. The actor on the `head_ref_force_pushed` event is `cursor[bot]` (the cloud agent's push), not `cestercian`.
- Barnacle reacted to the push within seconds (relabeled `size: M` to `size: L` at 07:15Z), so it saw the new head, but it did **not** remove `stale`. As of 2026-10-28 ~04:20Z (about 21 hours later, past its next ~03:55Z sweep) the label is still on.
- Contrast with the v1 era: on 2026-10-18 a `cursor[bot]` force-push at 04:16Z was followed by Barnacle removing `stale` at 15:36Z the same day.

## Reading (hunch, not confirmed)

- v2's stale comment says "qualifying **human** activity" resets the clock. A push whose event actor is a bot account apparently doesn't qualify under v2, even though it is a new head. Under v1 the same kind of push did clear the label.

## Practical rule

- For OpenClaw keep-alive, the push or a follow-up must come from the human account: push the rebased head as `cestercian` (not via the cloud agent's app token), or add a substantive human comment on the PR after a bot push.
- `#158348` close deadline unless something human happens: ~2026-11-03 03:55Z.
