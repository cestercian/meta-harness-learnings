# 2026-10-05 — Omnigent stale-issue close, OpenClaw bot-mediated merge, Bridge release pipeline

Activity after the 2026-10-02 T3/OrcaRouter/Sotto note (commit `3d996b6`). Three posture signals and one release-architecture lesson; the rest of the weekend traffic was status only.

## Omnigent — claimed issue fixed underneath the PR (`#8144` closed, not merged)

On 2026-10-02 `omni-resolve-agent` closed `#8144` (probe-home credential symlink refresh) with an `omnigent-stale-linked-pr` marker:

- Omnigent's repro agent re-ran issue `#6252` against current main, found every symptom gone, and closed it as already fixed (upstream commit `7c0c22a4b`).
- The internal Linear ticket (OMNI-6010) went to Done, and the bot then auto-closes any open PR whose linked issue or ticket is no longer open.

**Contrib posture:** on Omnigent, an open issue is not a stable claim. Before (and while) a PR is open, re-check that the linked issue is still open and still reproduces on main; a maintainer-side fix lands without touching your PR, and the bot closes yours with no review. The design lesson (unlink and rematerialize credential symlinks every probe) still stands; the PR died on overlap, not on design.

## OpenClaw — merges are bot-executed after maintainer-driven ClawSweeper loops (`#162033` merged)

The Windows `\\?\` session-publication fix merged on 2026-10-03, executed by `roboclaw-bot`, not a human. Before that, maintainer RomneyDa posted `@clawsweeper re-review` roughly nine times over 2026-10-01 → 10-03; each run either completed a durable review or was superseded by a newer review tuple.

**Contrib posture:** ClawSweeper re-reviews are maintainer-triggered and can take several passes; the contributor does not need to push or ping during that loop. Merge readiness is whatever the latest durable ClawSweeper verdict says, and the merge itself is automated. Contrast `#158348`, where obviyus' "main doesn't repro it" stands and CI cleanup alone does not unblock it (prove-on-main rule from 09-28 still applies).

## T3 — backend fixes do merge on Macroscope approve + maintainer (`#13295` merged)

`#13295` (second server must not resend peer `thread.turn-start-requested` events) merged 2026-10-02 by t3dotgg after a Macroscope approvability review. Read together with the 10-01 UI closes (`#12890`/`#12906`): backend fixes with focused tests get through; UI fixes without before/after captures get closed. The evidence bar tracks the surface, not the bot verdict.

## Bridge — release pipeline shape (`#638` merged after ~3 weeks)

The release-automation PR that had been open since 09-14 merged 2026-10-02. The design is reusable for any signed desktop harness:

| Concern | Choice |
| --- | --- |
| Versioning | One cumulative Release Please PR driven by validated conventional squash titles |
| Publish | Tag creation, macOS signing, notarization, mounted-DMG smoke and publication serialized in one protected-main run |
| Verification | Check versions, updater signatures, tag/SHA, release-PR provenance and the full remote asset set before publishing (upload to a draft first) |
| Credentials | Packaged-app and mounted-DMG acceptance jobs never see signing or publish credentials; mutation jobs mint short-lived repo-scoped GitHub App tokens |
| Lock sync | Release-PR lock synchronization runs tooling from protected main, never PR code, when App credentials are present |
| Nightly | Nightly releases preserved; prerelease strings stay out of extension manifests |

Long-lived release PRs need a current-main integration pass (title policy, stable workflow, lockfile versions bootstrapped from the last published tag) right before merge.

Also merged: supervised composer dictation (`#683`, previously draft pending live mic smoke) and opt-in edit-activity auto-expand (`#747`).

## Status only (no new architecture lesson)

- Orca-Code-Review `#54` still open with no review; gentle ping posted 10-03.
- OpenClaw `#158357`, Omnigent `#8312`, NeMo Agent Toolkit `#2226`: open, quiet.
- Prime: still vouch-gated; no new activity.

## Links

- https://github.com/omnigent-ai/omnigent/pull/8144
- https://github.com/openclaw/openclaw/pull/162033
- https://github.com/pingdotgg/t3code/pull/13295
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/638
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/683
