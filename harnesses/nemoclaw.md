# NemoClaw (NVIDIA)

- Repo: https://github.com/NVIDIA/NemoClaw
- Role: run agents inside OpenShell with managed inference (local Ollama/vLLM and cloud routes).
- Related: [OpenShell](https://github.com/NVIDIA/OpenShell) (secondary unless asked); pairs with NeMo Agent Toolkit when working NVIDIA surfaces.
- Contrib: **hard cap of 5 open PRs** — surplus opens are auto-closed by `github-actions`. Land or close before opening the next. Prefer scoped fail-loud fixes with live CLI/policy tests. Aging correct PRs often land as a maintainer replay on current `main` with co-author credit (`#12968`, `#12983`), so keep the regression test minimal and sharp.

## Notes for contributors

- MCP credential probe: use the adapter runtime (Node fetch / urllib), not curl on the generated binary allowlist; `#12104` superseded by maintainer salvage `#12175` merged (closes #12065).
- Backups: do not swallow permission-denied dirs then claim a full archive (#12108, closed). `#12340` replaced selective backup/audit with complete native-home/workspace capture and retired the snapshot CLI; incomplete archives fail and are removed before publication. The early-return **union** lesson still applies to collectors elsewhere.
- Local Ollama/vLLM: fresh managed OpenClaw defaults to **direct** tool disclosure — nested tool_call/Tool Search breaks those models. Landed via maintainer salvage `#12968` (Cestercian co-author); original `#12110` closed. Explicit CLI/env and existing sandbox choices stay authoritative.
- Policy remove verification: compare OpenShell readback through the same `_provider_*` stripping boundary used before writes, while still failing on any missing/changed operator-owned rule. Landed via maintainer replay `#12983` (Cestercian co-author); original `#12111` closed, fixes #12048.
- OpenClaw `devices approve` local fallback: drain stdout/stderr then `exit(0)` after Approved so leftover gateway handles do not hang `nemoclaw connect`. `#12109` superseded by maintainer salvage `#12178` merged (closes #12064).

- Backup restore: each backup writes a `.sandbox` marker; `restore <name>` without a timestamp only picks matching markers and fails if none match. Legacy unmarked backups need an explicit timestamp (`#12539`).
- Corporate CA (`NEMOCLAW_CORPORATE_CA_BUNDLE`): bundle must not be group-writable; `0664` fails onboarding at step 6/8 with a CA-named permission error, `0644`/`0600` work (v0.0.131, from the `#12059` re-check). `#12059` closed as no longer reproducing; re-verify old claims on the latest release before working them.
