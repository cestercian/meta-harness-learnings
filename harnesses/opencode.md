# OpenCode

- Repo: https://github.com/anomalyco/opencode
- Role: open-source coding agent (CLI/TUI/server); used as a provider behind control planes like T3 Code and Bridge.
- Contrib: large active tree; many labeled `bug` / `2.0` / `core`. Check for existing PRs before claiming — several high-visibility bugs already have closed or open fixes. Prefer PR template + linked issue.

## Notes for contributors

- Prefer accepted/core bugs with deterministic repros over intermittent provider flakes.
- Recurring seams (see 2026-09-18 batch): register-time opaque tool validation (#49573 / #35963); MCP mutation vs ToolRegistry reconcile fence (#49628 / #39902); attachment path labels in model context (#49631); undo of admitted-but-unpromoted input after interrupt (#49636); TUI background-hint gating vs already-backgrounded children (#49637).
- Second wave (2026-09-21): rewrite `0.0.0.0`/`::` service URLs to loopback for clients (#49805); alias pre-2.0.9 theme tokens (#49937); default omitted `capabilities.tools` for custom providers (#49940); trailing-slash OAuth issuer equivalence (#50046); foreground `serve` listening marker for VS Code (#50047); ErrorBoundary around plugin Slot (#50048); Windows prefer `.cmd` over nvm `.ps1` for npm/npx (#50050). `#49807` closed/obsolete — the binary is just `opencode`; the dual-name / `opencode2` resume-hint path is no longer needed.
- Cross-link with T3/Bridge when the bug is control-plane ↔ agent boundary.