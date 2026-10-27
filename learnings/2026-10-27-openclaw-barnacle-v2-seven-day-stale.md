# 2026-10-27 — OpenClaw `#158348`: Barnacle stale policy tightened to 7 + 7 days

## Contrib posture change: `openclaw-barnacle` v2

- `#158348` was rebased and pushed on 2026-10-19 03:36Z (head `9f05d5a`). Barnacle marked it stale again on 2026-10-27 03:55Z, about **8 days** later. The previous marker (2026-10-17) came roughly two weeks after the last push, so the window has shrunk.
- The marker is now `barnacle-stale:v2 item=<n> kind=pull-request lane=unassigned head=<sha>` (was `barnacle-stale-pr:v1 head=<sha>`). The comment text states the policy explicitly:
  - stale after **7 days without qualifying human activity**;
  - may be **closed as not planned 7 days after** the stale comment;
  - a human update **or a new PR head** restarts the clock;
  - maintainers can add `no-stale` (durable hold) or `bad-barnacle` (suppress).
- `lane=unassigned`: PRs with no maintainer assignee sit in the lane Barnacle sweeps. `status: needs proof` still does not protect a PR.

## Practical rule

- For an OpenClaw PR we want to keep open, plan a meaningful touch (rebase + focused re-run + push, or a substantive reply to review) at least once a week; otherwise the effective lifetime of an idle PR is ~14 days from the last head.
- `#158348` close deadline if nothing happens: ~2026-11-03 03:55Z.
