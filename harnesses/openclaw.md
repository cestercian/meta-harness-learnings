# OpenClaw

- **Repo:** https://github.com/openclaw/openclaw
- **Role:** Open-source multi-channel agent product (the harness we already contribute to).
- **License:** MIT (verify upstream).
- **Why it is “ours” today:** Prior upstream work (e.g. outbound sanitize / delivery paths); familiar CI (vitest suites, ClawSweeper durable reviews).
- **Gotchas:** Unrelated flakes in compact node suites; ClawSweeper wants real channel proof for some delivery paths; keep changes scoped and evidence-backed.
- **Contribution tips:** Prefer issue→PR with repro; expect bot review markers; babysit comments aggressively. Merges are executed by `roboclaw-bot` once the latest durable ClawSweeper verdict clears; maintainers trigger `@clawsweeper re-review` themselves (can take many passes), so no need to ping during that loop.
- **Prove-on-main:** if a new lifecycle regression already passes on unmodified main (and captured runtime stays healthy through the refused path), do not land the production change without a concrete repro — ClawSweeper and humans will block (`#158348`).
- Twilio Say fallback without media streams still needs a redacted after-speak call trace before merge (`#158357`).

## Idle / path gotchas (2026-10-01)

- Completions idle watchdog must re-arm on **model progress** (non-empty delta, reasoning, tools, finish, usage), not every parsed chunk — empty `"choices": []` keepalives otherwise stall until the full run timeout (`#162018`).
- Landed shape (2026-10-06): `#162018` was superseded by maintainer `#165983`, which keeps the documented connection-liveness watchdog (every valid chunk resets it) and adds a separate model-progress deadline at 2x the idle window. Do not change the meaning of a documented watchdog; add a second timer.
- `#165983` is the evidence template for `status: needs proof`: production-only negative control (tests unchanged, production files reverted to a named `origin/main` SHA, regressions fail for the intended reason) plus a real local HTTP/SSE trace with tiny windows.
- Windows session publication: strip `\\?\` extended-length prefixes before comparing SQLite DB paths to selected store paths (`#162033`); merged 2026-10-03 by `roboclaw-bot` after maintainer-driven ClawSweeper re-review loops.

