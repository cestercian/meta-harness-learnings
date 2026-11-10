# 2026-11-10 — Prime Agent: Rust port is the spec now; project-controlled code no longer auto-runs

**What happened.**
- `PrimeIntellect-ai/prime-agent#3870` (merged 2026-11-09 ~21:55Z, -5690 lines across 42 files) declared the Rust port complete: TS v0.9.8 is **no longer the spec**. It deleted the TS differential tests, the TS-frame parity scripts, the `pa-ai` TS-driver harness, the CI shard step that downloaded the v0.9.8 release, the ACP `compare_differential.py` captures, and the TS-parity policy in contributor docs (`AGENTS.md`, PR template, `.cursor/BUGBOT.md`).
- `#3907` (merged 2026-11-09 ~18:53Z) blocks project-controlled code at session startup: repo settings that pick shells, npm, MCP servers, or packages are ignored; project package installs are rejected (removal still allowed); project Python skills are treated as Markdown; `PYTHONPATH`/`PYTHONHOME`/`PYTHONUSERBASE` are stripped, the user site is disabled, and uv ignores config / interpreter-selection overrides and builds venvs from managed Python. It was filed as a deliberate security departure from v0.9.8 behavior.

**Learning.**
- **PR evidence bar changed.** Vouched PRs no longer need a "parity-diff vs TS" section. The template is now ownership compliance (crate scope + dependency direction per `AGENTS.md`), fmt/clippy/test gates, a `.changes/<branch-slug>.md` fragment (or `no-changelog` label), and test/e2e evidence. Don't cite TS behavior as the reference for a fix anymore; cite Rust tests.
- **Trust boundary pattern.** Opening a repo is now treated as untrusted input: config can set preferences but can't pick executables. This matches the fail-closed direction elsewhere (Bridge credential policy, OpenClaw sandbox). For contributions: any fix that reads project-level settings must not reintroduce an executable path, and Python-skill or MCP fixes have to work with project activation off.
- Intake posture is unchanged (Discussions first, vouched only, ENG/RES ticket gate).
