# 2026-10-19 — OpenClaw `#158348`: stale bot, rebase-to-keep-alive, reload status overlay

Activity since the 2026-10-16 note (`b9570bb`). Prime, T3, OrcaReplay, Orca-Code-Review, OrcaRouter-Lite, hermes-agent, Omnigent and NemoClaw had no new cestercian PR activity. OpenClaw `#158348` is the only harness PR that moved.

## Contrib posture: `openclaw-barnacle` stale marker

- `openclaw-barnacle` marked `#158348` stale on 2026-10-17, about two weeks after our last push/comment (2026-10-03). Its marker comment embeds the head SHA (`barnacle-stale-pr:v1 head=<sha>`), so it is keyed on branch inactivity, not on review state.
- A `status: needs proof` PR still goes stale; the label does not protect it.
- Keep-alive that actually changes something: rebase onto current `main`, rerun the focused suites, and push. Do not post a bare "bump" comment.

## Fix pattern: rebases surface main-side interactions

Rebasing onto main (2026-10-18) broke a plugin-reload recovery test. The fix (`9f05d5a`, `fix(gateway): keep failed plugin reload status after reserved recovery`) is a general status-overlay lesson:

- A rejected reservation kept reporting a "recovering" status after `pending` cleared, which reopened the transient queued-status overlay.
- `finishReload` then wrote the **failed** outcome onto that overlay instead of the baseline. A later successful sibling reload snapshotted the overlay baseline and silently dropped the failed plugin from readiness.
- The fix has two parts. (1) While a replacement generation is reserved, route `reportReloadStatus` through `replacement.setReloadStatus`, so status lands on the generation that owns it. (2) After a reject, close any leftover transient overlay (`closeTransientReload(false)`) before recording the outcome, so the terminal outcome is what later reloads snapshot.
- Rule of thumb: terminal outcomes (failed, unchanged) belong in the baseline, and only queued/admission state belongs in the transient overlay. Close the overlay before writing a terminal status.

CI on the new tip: 5 pass, 1 pending, 4 skipping, no failures at the time of writing. The PR is still proof-gated: maintainers want a concrete reload trigger that reproduces on unmodified main.

## Links

- https://github.com/openclaw/openclaw/pull/158348
- https://github.com/openclaw/openclaw/commit/9f05d5ae
