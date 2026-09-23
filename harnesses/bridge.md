# Bridge

- **Repo:** https://github.com/Atharva-Kanherkar/bridge-harness
- **Role:** Native macOS control room for coding agents (Codex, Claude Code, OpenCode, Cursor, Grok) with isolated task workspaces and provider usage visibility.
- **Peer:** Closest to **T3 Code** among harnesses we track (multi-provider control surface); Bridge is Mac-native rather than T3’s multi-client stack.
- **Site:** https://bridge.agentclash.dev
- **Contribution posture (2026-09):** Active — native usage menu (`#628`) and packaged-daemon CI smoke (`#633`, closes `#555`) merged; Cursor usage-cache discipline shipped in `#639`; release automation `#638` still open. Treat release/signing CI carefully (credential-free smokes).
- Unknown `$harness` shortcuts are rejected with available-ID hints, not sent as chat (`#678` merged).
- Usage ledger: price and attribute the **serving** model; session and prompt models stay what Bridge asked for. Flipping `usage_ledger.model` without a backfill mixes requested-model and serving-model rows (`#682` merged).
- Warm workers: send only the variable prompt suffix to compatible hot workers; reset stale context gauges with the durable SQL watermark `context_usage_after_id` (not wall-clock) on model, harness, or provider-binding changes (`#682`).
- Anonymous tools: keep empty anonymous starts out of visible activity until identity, output, or a terminal result; keep recognized subagent cards (`#682`).
- Degraded replay: unreadable stored entries become `entry.invalid` carriers at the original sequence (`status: degraded`) so pagination and reconnect continue (`#682`).
- CI: immutable committed `fake-gh.sh` fixtures avoid Linux races that spawn a newly written executable (`#682`).
- Supervised composer dictation / sherpa-onnx (`#683`) is still a draft until live mic smoke.
- **See also:** `learnings/2026-09-14-bridge-harness-target.md`, `learnings/2026-09-15-bridge-packaged-smoke-and-usage-cache.md`, `learnings/2026-09-23-bridge-usage-replay-nemoclaw-salvage-opencode.md`
