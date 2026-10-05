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
- ClawSweeper may ask to separate transport-liveness from a bounded model-progress deadline, and still wants redacted after-fix agent-flow proof.
- Windows session publication: strip `\\?\` extended-length prefixes before comparing SQLite DB paths to selected store paths (`#162033`); merged 2026-10-03 by `roboclaw-bot` after maintainer-driven ClawSweeper re-review loops.

