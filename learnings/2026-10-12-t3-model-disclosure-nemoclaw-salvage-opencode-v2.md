# 2026-10-12 — T3 model/harness disclosure cleanup, NemoClaw maintainer salvage, OpenCode V1→V2 closes

Activity after the 2026-10-09 NemoClaw backup-audit note (commit `d2ac4b6`). Three real contrib-posture shifts landed over the weekend; Sotto `#493` hit the same API-drift class as `#474`.

## T3 — disclose model and harness, or the PR gets cleaned up

On 2026-10-11, `maria-rcks` (notes written by `claude-opus-5-5` on her behalf) closed four of our still-open T3 PRs in one pass:

- `#12633` (service.env launcher merge)
- `#12863` (Windows DEP0190 `.cmd`/`.bat` spawn)
- `#12878` (Antigravity `.cmd` unwrap)
- `#12889` (Codex app-server JSONL byte ceiling)

The stated reasons: the PR description does not say which model or harness created it, and the PR is more than two weeks old (some also conflicted with `main`). There was a brief reopen-then-reclose cycle on each.

**Contrib posture:**

- Put the **model and harness** in the PR body up front (same spirit as the verification section). Macroscope APPROVED alone does not protect a PR from this cleanup.
- Stale PRs (>~2 weeks) without that disclosure are liable to be closed without merge. Rebuild on current `main` with a newer model and note both if the change still matters.
- This is separate from the 2026-10-01 UI-evidence closures (`#12890` / `#12906`): those needed screenshots; these need authorship metadata.

## NemoClaw — maintainer salvage vs architecture rewrite

Two of ours closed on 2026-10-10 without merging our branch, both with credit:

- **`#12110` → `#12968` (merged).** Progressive tool disclosure as the default breaks local Ollama/vLLM (nested `tool_call` / Tool Search). `ericksoa` replayed the narrow default onto current code as `#12968` (`fix(openclaw): default fresh local tool disclosure to direct`), credited Cestercian as co-author, and closed `#12110`. Explicit CLI/env choices and existing sandbox choices stay authoritative; only a fresh route-only reservation from onboarding gets the local-direct default.
- **`#12108` closed after `#12340`.** The incomplete-backup audit we fixed was replaced by `#12340` (complete native-home/workspace capture; snapshot CLI retired). Archive read failures now fail the backup and remove the incomplete archive before publication. The old permission-error-discarding audit path is gone, so the early-return union lesson from 2026-10-09 is historical for that call path even though the general "union collected failures on early return" pattern still applies elsewhere.

**Lesson:** On NemoClaw, a scoped PR can land as a maintainer salvage with co-author credit, or be overtaken by a larger rewrite. Either way, watch the close comment — do not reopen or re-push the old branch without checking what replaced it.

## OpenCode — V1 PRs rejected after V2 promote

`thdxr` closed several of our still-open V1-line PRs (including `#50048` ErrorBoundary around plugin Slot, `#50050` Windows prefer `.cmd` over nvm `.ps1`, `#49710`, `#49706`) with: the branch was upgraded to V2; V1 PRs are no longer accepted; port and open against V2 if still relevant (`<!-- v1-pr-dev-promoted-v2 -->`).

This pairs with the 2026-10-08 note that early-v2 *issues* can be obsolete: now open *PRs* against the pre-promote line are also dead. Before claiming or reopening OpenCode work, confirm the target branch is current V2 `main`, not a leftover V1 head.

## Sotto `#493` — same `notice.message` drift as `#474`

`Maxerns` verified the theme-save regressions (negative control: silent success fails both tests) but blocked: `#516` introduced `StatusNotice`, and four new assertions still destructured the old tuple shape. GitHub still showed mergeable. Same class of lesson as `#474` — rebase, switch to `notice.message`, rebuild locally before asking for re-review. Waiting on Maxerns after the rebase.

## OpenClaw status (no new lesson)

- `#158348`: `obviyus` confirmed the new lifecycle regression already passes on unmodified main; prove-on-main rule already recorded. Parked pending a concrete reload trigger.
- `#158357`: Twilio Say fallback still needs a redacted after-speak trace; formatting/export nits fixed.

## Links

- https://github.com/pingdotgg/t3code/pull/12633
- https://github.com/pingdotgg/t3code/pull/12863
- https://github.com/NVIDIA/NemoClaw/pull/12968
- https://github.com/NVIDIA/NemoClaw/pull/12110
- https://github.com/NVIDIA/NemoClaw/pull/12108
- https://github.com/NVIDIA/NemoClaw/pull/12340
- https://github.com/anomalyco/opencode/pull/50050
- https://github.com/getsotto/sotto/pull/493
