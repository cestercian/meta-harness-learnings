# 2026-11-25 — OrcaCode Review: "no newer sibling run" must ignore runs that can never settle

## What happened on `Continuum-AI-Corp/Orca-Code-Review#54` (fixes #39)

`#54` moves reaction settle (👀 → result) and the clean-verdict post onto one planner, `scripts/authority.mjs`: a run is authoritative for a head only if no sibling run on that PR has a higher run id. It sat quiet from 2026-10-22 until two pushes:

- `f6f87648` (2026-11-24): settle clears 👀 through the identity funnel (👀 create-response login → `/user` → `{app_slug}[bot]` → `github-actions[bot]`), under authority gating.
- `b2c646e8` (2026-11-25): ignore **cancelled** newer runs when picking the authoritative run.

The bot found a new P1 on each push. Both were the same bug in different forms.

1. **Cancelled siblings (P1, 2026-11-24).** Both callers (action.yml post step and settle step) cut each run down to `{id, name}`, which throws away `status`/`conclusion`. GitHub's `actions/runs` list keeps cancelled runs forever, and a hard cancel can kill a job before its `always()` settle step. So a newer run that dies early acts like a live one, and the older run, the only one that will ever settle that head, stands down for good. 👀 stays and no verdict is posted.
2. **Skipped siblings (P1, 2026-11-25, still open on `b2c646e8`).** The workflow runs on `issue_comment: [created]` and `pull_request_target: edited` and gates the real work with a job-level `if:`. Every ordinary PR comment or title/body edit starts a newer run on the same head that finishes as `completed/skipped` and never reaches settle. `siblingCanSettle()` only drops `cancelled`, so a chat comment still vetoes the real run.

## Learning (generalizes to any "latest run wins" CI gate)

- An authority rule of "highest id wins" needs a **can-this-sibling-still-settle** filter: keep `queued`/`in_progress`, and `completed` only with a conclusion that ran the settle step. Drop `cancelled`, `skipped`, and in practice `startup_failure`/`timed_out` before they reach the job.
- Pass `status` and `conclusion` through every caller. Cutting runs down to `{id, name}` at the call site is what hid the bug, so the planner never saw the data it needed.
- Any workflow that has comment/edit triggers plus a job-level `if:` makes skipped runs all the time, so test the planner against a skipped newer run, not only a cancelled one.
- Fixing one terminal state at a time costs a review round each push. List all the `conclusion` values once and decide each one.

## Status

- `#54` open, head `b2c646e8`, 1 P1 (skipped siblings) blocks merge. Next push: allowlist settle-capable states instead of denylisting `cancelled`, and add a skipped-sibling test.
- Other harness targets (Prime Agent, T3 Code, OrcaReplay, OrcaRouter-Lite, OpenClaw): no new cestercian activity since the 2026-11-24 note.
