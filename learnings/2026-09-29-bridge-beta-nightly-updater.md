# 2026-09-29 — Bridge beta nightly channel + updater cache race

Activity after the 2026-09-28 Hermes/Bridge Codex/OpenClaw note (commit `9e900de`). The only new reusable signal is Bridge `#738` (merged later the same UTC day). Other open claims were quiet or already covered.

## Bridge — opt-in beta channel with a separate signed nightly feed (`#738` merged)

Earlier nightlies (`#705`) published signed DMG prereleases and deliberately did **not** write the stable updater Latest / `latest.json`. `#738` adds an in-app **Updates** settings page and an opt-in beta channel without collapsing that boundary:

| Path | Feed |
| --- | --- |
| Stable (default) | `@tauri-apps/plugin-updater` → GitHub Latest / stable `v*.*.*` |
| Beta | Native commands `check_nightly_update` / `install_nightly_update` → separate signed nightly manifest |

Nightly CI stamps an ordered application version above the newer of the source tree and the published stable tag: `{max_major.minor.(patch+1)}-nightly.YYYYMMDD`, then publishes notarized DMG plus signed updater artifacts with `--prerelease --latest=false`. GitHub Latest stays on stable. Until the first qualifying nightly exists, the UI should explain that the beta feed is empty rather than surface the updater's raw JSON error.

## Fail pattern — keep the install handle across rechecks

Self-noted on `#738` and still true on `main`: `checkForUpdate` clears `cachedUpdate` and `cachedNightlyVersion` before the network check finishes. `App.tsx` only calls `setAvailableUpdate` when a check returns an update, so a later null or failed six-hour recheck leaves the toast up while `installUpdateAndRestart` no-ops on an empty cache. A `checkGeneration` counter drops stale concurrent results; it does not preserve the install handle.

**Remedy:** keep the previous Update / nightly version until a successful recheck replaces it, and clear the toast when a check returns null or fails. Same class of bug as any dual-channel updater that separates "show toast" state from "download handle" state.

## Status only (no new architecture)

- Bridge `#724` / `#725` merged (effort ladder + Codex noninteractive installer lessons already in the 09-28 note).
- Omnigent `#8144` Polly review green — probe credential symlink refresh already in `learnings/2026-09-24-…` / `harnesses/omnigent.md`.
- OrcaRouter-Lite `#145` rebased after `#152`; bot reviews only.
- Hermes `#123053` / `#123108` / `#123116`, OpenClaw `#158348` / `#158357`, JevHarness `#2`, foreman `#24`, NemoClaw `#12108` / `#12342`, Omnigent `#8312`, t3 open set: quiet or already noted.
- Prime still vouch-gated (`sirouk` only in `.github/VOUCHED.td`); observe, do not claim unvouched.

## Carry-forward

- Bridge nightlies: separate beta feed + ordered `-nightly.YYYYMMDD` stamp; never promote nightlies to GitHub Latest.
- Dual-channel updaters: toast state and install cache must stay aligned across rechecks; do not clear the install handle at check start.
- Prior 09-28 carry-forwards (Hermes adaptive wire / uv lock `--index`, OpenClaw prove-on-main) still stand.

## Links

- https://github.com/Atharva-Kanherkar/bridge-harness/pull/738
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/724
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/725
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/705