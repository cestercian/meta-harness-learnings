# Bridge

- **Repo:** https://github.com/Atharva-Kanherkar/bridge-harness
- **Role:** Native macOS control room for coding agents (Codex, Claude Code, OpenCode, Cursor, Grok) with isolated task workspaces and provider usage visibility.
- **Peer:** Closest to **T3 Code** among harnesses we track (multi-provider control surface); Bridge is Mac-native rather than T3’s multi-client stack.
- **Site:** https://bridge.agentclash.dev
- **Contribution posture (2026-09):** Active — native usage menu (`#628`) and packaged-daemon CI smoke (`#633`, closes `#555`) merged; Cursor usage-cache discipline shipped in `#639`; release automation `#638` still open. Treat release/signing CI carefully (credential-free smokes).
- **See also:** `learnings/2026-09-14-bridge-harness-target.md`, `learnings/2026-09-15-bridge-packaged-smoke-and-usage-cache.md`
