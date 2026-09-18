# OpenCode

- Repo: https://github.com/anomalyco/opencode
- Role: open-source coding agent (CLI/TUI/server); used as a provider behind control planes like T3 Code and Bridge.
- Contrib: large active tree; many labeled `bug` / `2.0` / `core`. Check for existing PRs before claiming — several high-visibility bugs already have closed or open fixes. Prefer PR template + linked issue.

## Notes for contributors

- Prefer accepted/core bugs with deterministic repros over intermittent provider flakes.
- Recurring seams (see 2026-09-18 batch): register-time opaque tool validation (#49573 / #35963); MCP mutation vs ToolRegistry reconcile fence (#49628 / #39902); attachment path labels in model context (#49631); undo of admitted-but-unpromoted input after interrupt (#49636); TUI background-hint gating vs already-backgrounded children (#49637).
- Cross-link with T3/Bridge when the bug is control-plane ↔ agent boundary.
