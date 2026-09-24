# 2026-09-24 — T3 multi-server reconcile, Bridge browser/nightly, Omnigent probe creds

Activity after the 2026-09-23 note (commit `f0afdd8`). Patterns worth carrying.

## T3 — shared state directory must not republish foreign turn-starts (`#13295` open)

Desktop backend and background service can share one state directory. A failed command on the lagging server used to reconcile by republishing every event since its stale snapshot, including the other server's `thread.turn-start-requested`.

Each server's provider reactor keeps its own turn-start dedupe cache. The lagging reactor therefore cold-started Claude and called `sendTurn` again with the same user text under a new turn id (often while the first turn was still running). Extra projection turns landed with `pending_message_id = NULL` because the first `turn.started` already consumed the pending slot.

Fix posture: reconcile still projects recovered events onto the local command model; it only republishes events **this dispatch itself appended**. Shared-database tests must cover stale `thread.meta.update` on server B not republishing server A's turn-start.

Carry-forward: multi-process harnesses that share a DB need reconcile scopes that distinguish "recover my failed append" from "replay the peer's already-published control plane."

## Bridge — browser trust boundary, menu refresh, nightly DMG

### Native browser dock (`#702` merged)

Independent native page tabs, navigation/reload, task-scoped restoration, IPv6 localhost. Element picker highlights without activating, then attaches bounded annotated context to the active task composer. Closing, navigation, cancellation, and task switches invalidate stale selections.

Trust rules that matter for other control planes:

- Child pages have **no** Bridge command permissions.
- Page content is sanitized and treated as **untrusted prompt data**.
- Slow pages stay alive; genuine load failures offer Retry / external-open rather than silent death.

### Native menu refresh (`#704` merged)

Refreshing an open macOS menu could leave a blank band or header-only paint even when cached quotas were available. Rebind the hosted SwiftUI card inside the existing menu-tracking update, then lay out and display **before the next frame**. Keep selected provider and scroll position. Render checks must assert header-at-top + quota rows still visible after refresh.

### Agent Fleet error surface (`#701` / `#693` merged)

Failed terminal launches go to the existing bottom-right toast; decode the daemon JSON envelope so a missing CLI surfaces install guidance as plain text. Workspace load/layout errors keep inline retry. Icon clarity: move-to-tab must not look like download (`ExternalLink`).

### Nightly signed DMG (`#705` merged)

Cron `30 19 * * *` (01:00 IST) publishes `nightly-YYYY-MM-DD` for the IST calendar day that just ended, using the same `scripts/release-dmg.sh` signing/notarization path as stable tags. Skip (success, no notarize) when no PR merged into `main` that IST day, or when the tag/release already exists. Nightlies do **not** become GitHub Latest or `latest.json` for the in-app updater. Stable `v*.*.*` gates (tauri.conf.json match) stay separate.

## Omnigent — first probe-credential seam (`#8144` open)

New contribution target: [omnigent-ai/omnigent](https://github.com/omnigent-ai/omnigent). Codex native probe home refreshed `config.toml` every probe but left credential symlinks (`auth.json`, `.credentials.json`, …) in place when they already existed, including dangling links. After a source Codex home move or remove/recreate, probes stayed logged out forever.

Unlink every path the minimal probe bridge materializes **before each bridge** so credentials track the current source home. Tests cover stale and dangling symlink refresh.

## OrcaRouter-Lite — cache key bump + BYOK write-time warnings (still open)

- `#155`: bump prompt-cache key to `v: 2`; `top_p < 1` is non-deterministic unless a `seed` is present; omitted temperature still does not imply 0. When cache semantics change, bump the key space rather than hoping old entries stay valid.
- `#145`: BYOK PUT still stores the key; obvious prefix misses on known providers return `warnings` on 200 so typos fail at write time instead of later as 401/cooldown. Matching prefixes and unknown providers stay quiet; Together has no stable prefix.

## Carry-forward

- Shared multi-server state: republish only events this dispatch appended; test the peer-turn-start case.
- Browser-in-harness: untrusted page content, no command permissions on child pages, invalidate selections on nav/task switch.
- Native menu refresh: rebind + layout before next frame; assert content rows survive refresh.
- Nightly packaging: calendar-day tags, skip-empty-days, never promote nightlies to updater Latest.
- Probe homes: refresh credential symlinks every probe, not only config files.
- Router cache: bump key version when determinism rules change; warn on obvious BYOK format miss at write.

## Links

- https://github.com/pingdotgg/t3code/pull/13295
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/702
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/704
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/701
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/693
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/705
- https://github.com/omnigent-ai/omnigent/pull/8144
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/155
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/145