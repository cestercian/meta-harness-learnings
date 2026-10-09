# 2026-10-09 — NemoClaw backup audit: keep nested failures on unsafe early return

Activity after the 2026-10-08 stale-claims / Sotto API-drift note (commit `d5f8410`). One real architecture fix on an open NemoClaw PR, plus a Sotto merge that closes the loop on yesterday's note. Everything else on the named harness targets was quiet.

## NemoClaw `#12108` — early return must union, not replace, collected failures

PR goal: report unreadable backup directories as failures (with reasons) instead of swallowing them and claiming a full archive.

`collectPreBackupAuditViolations` already records nested `u` (unreadable) entries into `failedDirs` / `failedDirReasons` before returning any unsafe-entry list (`l` / special file). The caller's unsafe-entry early return then used `failedDirs: [...existingDirs]` (declared roots only) and omitted `failedDirReasons`. When the same audit found both `workspace/restricted` (permission denied) and a disallowed symlink under the same declared root, the backup correctly failed, but the result only listed `workspace` with no permission reason. An operator could fix the symlink and only then discover the nested permission problem on the next attempt.

**Veridical** (`lukaszszafranski`) caught this on 2026-09-27; we replied and landed the fix (commit `ac79896`): preserve the collected nested paths and reasons, still mark every declared root affected by the unsafe entry, and add a mixed-audit regression (nested unreadable + unsafe symlink together). Separate unreadable-only and unsafe-only tests were not enough.

**Lesson:**

- When an early return short-circuits after a collector has already recorded partial failures, **union** those records into the returned result. Replacing the array with a coarser declared-root list drops the detail this PR exists to surface.
- Cover the **combination** of audit conditions in tests, not only each condition alone. Mixed cases are where early-return wipes show up.

## Sotto `#474` merged

`Maxerns` approved and merged `#474` on 2026-10-08 after the `#516` `StatusKind` rebase noted yesterday. Negative-control verification (removing the collision check fails the new regression) stood. No new lesson beyond "rebase + rebuild before re-review on stale PRs."

## Status only

- OpenClaw `#158348`: still open; CI noise (cancelled/skipped auto-response), no new human review.
- T3 `#12633`, NemoClaw `#12110`/`#12111`/`#12342`/`#12539`, OrcaRouter-Lite `#166`, Orca-Code-Review `#54`, Hermes, Omnigent, Prime, OrcaReplay: no new review or merge activity on our PRs since the last note.

## Links

- https://github.com/NVIDIA/NemoClaw/pull/12108
- https://github.com/getsotto/sotto/pull/474