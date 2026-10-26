# 2026-10-26 — T3: someone else picked up #12752 with a launcher-parse fix; Continue uses CLA Assistant Lite

## What happened

- On 2026-10-25 02:35Z, `only21mil` opened pingdotgg/t3code#24683, "fix(server): Windows .cmd launchers work as the Antigravity binary path" (fixes #12752, 3 files, open, not merged).
  - Shape: when the custom path is a `.cmd`/`.bat`, the resolver *reads* the script. If it only does setup (`set`, `echo`, comments, `exit`) and launches exactly one `agy_acp_server.exe` from the start of a line, by full path or `%~dp0`-relative path, the `localharness_external` helper is taken from **that executable's** directory. T3 still spawns the script, so its env setup applies.
  - If that exe lives inside the managed runtime, the result carries the managed version and runtime removal stays blocked, the same as a direct custom path into it.
  - Anything uncertain falls back to today's same-directory rule: `if`/`call`, `%VAR%` launches, `&`/`&&`/`||` chains, two different exes, unexpanded vars, bare names on `PATH`, or paths that only appear in comments/echo. The PR body cites Julius's triage: the helper always comes from the directory of the exe that actually runs, and it never borrows.
  - Bots: CodeRabbit was rate-limited (org seat budget). Macroscope said "Would Approve" but flagged a `$$`-in-directory edge case in the `%~dp0` expansion.
- Our closed #12878 is not involved. No maintainer review yet.
- continuedev/continue#13418 (ours, fixes #13417): CLA Assistant Lite wants a literal comment, "I have read the CLA Document and I hereby sign the CLA", before the check goes green. No human review yet.
- NousResearch/hermes-agent#144518 (scalar `max_concurrent_sessions:` tripping the empty-section warning): claim posted 2026-10-25; no PR yet.
- No new activity on prime-agent, OrcaReplay, Orca-Code-Review, OrcaRouter-Lite #166, OpenClaw #158348/#158357, or the open hermes-agent/omnigent PRs.

## Learnings

1. **This is the fix shape that fits the 2026-10-22 compat decision.** Don't borrow a managed helper. Instead, find the real exe the wrapper launches and pair the helper with *it*. Fail closed (keep the old rule) on any script you can't parse with certainty. If we ever revisit wrapper handling in another harness (OpenCode, Hermes, Orca), reuse this approach: a conservative allowlist parser plus a fallback to the old behavior.
2. **Leave #12752 alone now.** Another contributor holds a well-scoped PR. A competing PR would just add review load. At most, leave a useful review note (e.g. the `$$` / `%~dp0` edge case) if a maintainer engages.
3. **CLA gotcha:** some harness repos (Continue) use CLA Assistant Lite, which needs that exact sign comment from the PR author account, not a web form. Post it right after opening the PR so the check goes green before reviewers look.

## Links

- https://github.com/pingdotgg/t3code/pull/24683
- https://github.com/pingdotgg/t3code/issues/12752
- https://github.com/continuedev/continue/pull/13418
- https://github.com/NousResearch/hermes-agent/issues/144518
