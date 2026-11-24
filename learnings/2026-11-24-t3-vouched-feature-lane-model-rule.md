# 2026-11-24 — T3 Code: vouched contributors get a feature lane; Gemini 4 Neon accepted

## What changed in `pingdotgg/t3code` CONTRIBUTING.md

- **`#38456` (2026-11-18): vouched contributors can open small feature PRs.** Anyone in `.github/VOUCHED.td` can open a small, focused feature PR without a maintainer asking for it. The rest of the auto-close list still holds: UI overhauls, new providers, architecture overhauls, source control, auth, and platforms.
- **Feature lane cap:** at most **3 open feature PRs** per vouched contributor. A fourth is closed. Bug-fix PRs don't count toward it.
- **`#38893` (2026-11-19): model rule loosened.** It used to be "Gemini (any version) or Muse Wonder wrote any code → closed". Now Gemini 4 Neon and Muse Wonder Full are accepted. Other Gemini models and plain Muse Wonder are still closed. Naming a model only as a reviewer is still fine. "We use only the model the PR names" is unchanged, so the model disclosure in the PR body is what the rule checks.
- The `docs/operations/` runbook path moved (`#41330`, 2026-11-23: `00_development.md`). The setup link changed, the rules did not.

## Rules we hadn't written down

- **3 open PRs per person** (non-vouched). A fourth is closed when it's opened.
- **`needs-rebase` → closed** after 7 days of conflicts with no activity.
- **Closure notice:** a rule-breaking PR gets a notice that names the rule and closes 72h later. A reply or a push inside that window sends it back for another look.
- **Vouched contributors are exempt** from the 3-open limit and the conflict window. The feature cap above still applies to them.

## Our standing

- `github:cestercian` has been in `VOUCHED.td` since `f9e8f578fa` (`chore: vouch cestercian`, `#12638`, 2026-09-19) and is not denounced. We have **0 open PRs** on t3code as of 2026-11-24.
- So we can do bug fixes (no count limit) plus up to 3 small feature PRs. Each one still needs the verification bar: how the bug was found and checked, `scenarios:prove` for web-reproducible bugs (2026-11-11 note), and before/after media for UI. Model + harness disclosure is still required.
- Bans still apply: "spam us with automation" and "repeated pings" are both ban reasons. Keep the nudge rule (one ping per maintainer per week) strict here, because a denounce shuts down the whole repo for us.

## Elsewhere this week

- Prime Agent: only an `AGENTS.md` prose reflow (`#4131`). Intake unchanged.
- OrcaRouter-Lite `#166` (LiteLLM Router timeout), JevHarness `#2`, continue `#13418`, claude-agent-sdk-python `#1259`: our nudges only, no maintainer replies. Omnigent `#8312`: no change since the 2026-11-18 note.
- OpenClaw: PR template now asks for the problem before the fix (2026-11-20). Not acting on it, per the no-OpenClaw-PRs rule.
