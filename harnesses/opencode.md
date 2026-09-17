# OpenCode

- Repo: https://github.com/anomalyco/opencode
- Role: open-source coding agent (CLI/TUI/server); used as a provider behind control planes like T3 Code and Bridge.
- Contrib: large active tree; many labeled `bug` / `2.0` / `core`. Check for existing PRs before claiming — several high-visibility bugs already have closed or open fixes.

## Notes for contributors

- Prefer accepted/core bugs with deterministic repros over intermittent provider flakes.
- Runtime MCP mutation vs ToolRegistry reconciliation (#39902) and stream/tool-argument contracts are recurring themes.
- Cross-link with T3/Bridge when the bug is control-plane ↔ agent boundary.
