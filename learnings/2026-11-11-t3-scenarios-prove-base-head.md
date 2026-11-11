# 2026-11-11 — T3 Code: `scenarios:prove` is now the proof for web-reproducible bugs

**What happened.**
- `pingdotgg/t3code#34007` (merged 2026-11-10 ~23:45Z) added **scenarios**: one-test-per-file e2e specs in `scripts/scenarios/*.e2e.ts` that boot the real dev stack (real server, HTTP, WebSocket) on a fresh T3 home, with the dev replay agent standing in for the model, and drive a real browser (web) or a pooled iPhone simulator (mobile).
- `vp run scenarios:prove <file> --base <rev> --head <rev> [--upload-command "ghdrop upload"]` runs the same scenario on both commits (temp worktree per side), pairs `app.screenshot("label")` captures by label, and emits `proof.json` plus a Markdown base/head capture table ready for a PR body. Exit 0 only if it **fails on base and passes on head**; 1 = ran but didn't prove; 2 = couldn't run (missing control or app didn't boot counts as "couldn't run", not as the bug).
- `AGENTS.md` proof rules gained one line: for a bug a user can reproduce in the web client, `scenarios:prove` is the expected proof. Logic/state/protocol bugs still get a failing unit test first; purely visual nits still get before/after captures and no test; server-only fixes still ship test + runtime transcript.

**Learning.**
- **Evidence bar moved from hand-made captures to a reproducible fail-on-base/pass-on-head run.** The PR's own framing: hand-built proof "is where most automated fix attempts die". For our T3 UI PRs (the ones closed in Oct for missing UI evidence), attach the `scenarios:prove` Markdown table instead of ad-hoc screenshots.
- Gotchas: exactly one test per file; only `expect` assertions on the broken thing; no `agent.act`/`agent.assert` (no model configured); one run per checkout at a time (it recreates the scenario T3 home and `scripts/.e2e`), so use a second worktree for parallel proofs. The scenario and e2e come from your checkout, so a new scenario can prove against a base that predates it.
- Cross-harness pattern: same direction as Prime Agent dropping TS-parity diffs for Rust tests (2026-11-10) — maintainers are codifying machine-checkable proof formats aimed at agent-authored PRs.
