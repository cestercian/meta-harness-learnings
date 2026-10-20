# OpenCode

- Repo: https://github.com/anomalyco/opencode
- Role: open-source coding agent (CLI/TUI/server); used as a provider behind control planes like T3 Code and Bridge.
- Contrib: large active tree; many labeled `bug` / `2.0` / `core`. Check for existing PRs before claiming — several high-visibility bugs already have closed or open fixes. Prefer PR template + linked issue.

## Notes for contributors

- Prefer accepted/core bugs with deterministic repros over intermittent provider flakes.
- Recurring seams (see 2026-09-18 batch): register-time opaque tool validation (#49573 / #35963); MCP mutation vs ToolRegistry reconcile fence (#49628 / #39902); attachment path labels in model context (#49631); undo of admitted-but-unpromoted input after interrupt (#49636); TUI background-hint gating vs already-backgrounded children (#49637).
- Second wave (2026-09-21): rewrite `0.0.0.0`/`::` service URLs to loopback for clients (#49805); alias pre-2.0.9 theme tokens (#49937); default omitted `capabilities.tools` for custom providers (#49940); trailing-slash OAuth issuer equivalence (#50046); foreground `serve` listening marker for VS Code (#50047); ErrorBoundary around plugin Slot (#50048); Windows prefer `.cmd` over nvm `.ps1` for npm/npx (#50050). `#49807` closed/obsolete — the binary is just `opencode`; the dual-name / `opencode2` resume-hint path is no longer needed.
- Cross-link with T3/Bridge when the bug is control-plane ↔ agent boundary.
- Issues filed against the early v2 pre-release (mid-2026) may be obsolete: `#36766` (truncated OpenAI tool-call args) was closed as not planned on 2026-10-08. Check version line and reproduce on current `main` before claiming.
- After the V2 promote, open PRs against the old V1 line are rejected (`thdxr`: port to V2). Examples closed 2026-10-10: `#50048`, `#50050`, `#49710`, `#49706`. Do not reopen a V1 head; retarget current V2 `main`.
- The **OpenCode Beta** VS Code extension (`sst-dev.opencode-v2`) is unmaintained; use `sst-dev.opencode` v1.0.0 with the V2 CLI. `#50043` closed 2026-10-14; don't target the Beta extension.
- **Automated PR cleanup** (from 2026-10-19): `github-actions` closes PRs that are over 1 month old with fewer than 2 positive reactions, whatever their review or CI state. `#49937`/`#49940`/`#50046`/`#50047` closed this way; all had also ignored the `pr-standards:issue` linked-issue warning. Link an issue (`Fixes #N`) at open time. To revive one, comment with a reason and a maintainer decides.
