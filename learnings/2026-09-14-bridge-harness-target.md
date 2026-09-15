# 2026-09-14 — New harness target: Bridge

## What it is

[Atharva-Kanherkar/bridge-harness](https://github.com/Atharva-Kanherkar/bridge-harness) — **Bridge**, a native macOS control room for coding agents (Codex, Claude Code, OpenCode, Cursor, Grok) on isolated task workspaces. Closest peer in our map is **T3 Code** (multi-provider control surface); Bridge is Mac-native (Rust + AppKit/SwiftUI) rather than T3’s web/desktop/mobile stack.

Site: https://bridge.agentclash.dev

## Why it’s on the map

Active contribution stream (open as of 2026-09-14), not drive-by:

| PR | Status | Focus |
| --- | --- | --- |
| [#567](https://github.com/Atharva-Kanherkar/bridge-harness/pull/567) | open (as of 2026-09-14 note) | GitHub links in transcript: ask open-in-pane vs browser; prose autolink |
| [#628](https://github.com/Atharva-Kanherkar/bridge-harness/pull/628) | **merged** | Native macOS usage menu + multi-provider overview (limits / cost / stale) |
| [#633](https://github.com/Atharva-Kanherkar/bridge-harness/pull/633) | **merged** (closes `#555`) | CI smoke: packaged macOS daemon lifecycle (credential-free, isolated) |
| [#638](https://github.com/Atharva-Kanherkar/bridge-harness/pull/638) | open | Automate Bridge release pipeline (Release Please + signed/notarized publish) |
| [#639](https://github.com/Atharva-Kanherkar/bridge-harness/pull/639) | **merged** | Drop retired usage widget; Cursor optional-metadata + stale-cache refresh |

Follow-up detail: `learnings/2026-09-15-bridge-packaged-smoke-and-usage-cache.md`.

## Contrib posture (early signal)

- Early-stage, Apple Silicon–first; release/signing CI is a real surface (smoke must stay credential-free and isolated from notarization jobs).
- Product shape overlaps T3’s “orchestrate many agent CLIs” job — useful for cross-harness comparison on usage meters, GitHub deep-links, and packaged-daemon health.

Treat as a living target alongside T3 / Orca / OpenClaw / Prime; keep notes here when PRs land or posture hardens.