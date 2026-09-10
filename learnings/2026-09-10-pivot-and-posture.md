# 2026-09-10 — Pivot to harness OSS + notes repo

## Decision
Prioritize contributions around agent harnesses (Prime Agent, T3 Code, Orca*, OpenClaw) and keep durable notes in `cestercian/meta-harness-learnings`.

## Process learnings (carry forward)
- **Fork-first** for Cursor cloud agents when launching on a new upstream org fails with `unauthenticated`.
- **Author as cestercian only** — strip AI `Co-authored-by` and Cursor PR footers before/at upstream open.
- **Parent reviews** full diff + tests + commit message before `gh pr create` upstream.
- **Babysits** escalate every human/bot comment; delete on upstream merge/close; never put `pr-merged`/`pr-closed` only on the fork listener.
- **CLA portals** (F5, CLA Assistant) need a human one-time sign — surface links immediately.
- **Prime Agent** is discussion-first / vouched-PR-only — do not spam PRs; use Discussions and wait for invite.
- **OpenClaw** remains the OSS harness already in our contribution orbit.

## First wave targets
- T3 Code issues labeled `accepted` (e.g. macOS LaunchAgent / Homebrew Node path pinning).
- OrcaReplay small bugs (e.g. ignored `--model` on TLS-intercept fork).
- OpenClaw continued when natural bugs appear.
- Prime Agent: Discussions + investigation until vouched.
