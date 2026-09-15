# 2026-09-15 — Bridge: packaged-daemon smoke + Cursor usage cache discipline

Our merges on [Atharva-Kanherkar/bridge-harness](https://github.com/Atharva-Kanherkar/bridge-harness) after the 2026-09-14 target note.

## Packaged-app smoke is a real release gate (`#633`, closes `#555`)

Standalone macOS CI job:

- Builds app-only, ad-hoc-signed `Bridge.app` with updater artifacts disabled — **no** release, signing, or notarization credentials.
- Launches the exact packaged executable under isolated home / data / temp / allowlisted env.
- Verifies bundled daemon start + authenticated health RPC against the isolated DB, then graceful shutdown and socket cleanup.

Native control-room harnesses need this split: credential-free packaged lifecycle smoke ≠ notarized publish. `#638` (still open) keeps both smokes isolated from signing jobs while automating Release Please + signed publish.

## Usage refresh: optional metadata and transient failure (`#639`)

Retired the in-app circular usage widget; native menu + Usage screen remain. Cursor collector posture (CodexBar-like):

- Optional account-label must not gate the required usage-summary fetch (fetch concurrently; shorter timeout on metadata). Valid summary still refreshes when the label request fails.
- Temporary transport / rate-limit / server failures retain the last reading **only** when the current unexpired Cursor session matches the cached account; mark stale and keep original observation time.
- Rejected auth, missing/changed local identity, or malformed responses do **not** authorize cache reuse. Account mismatch still rejects the reading.

## Carry-forward

- For Mac-native agent control planes: ship a credential-free packaged daemon smoke before trusting release CI.
- For provider usage meters: separate optional metadata from required quota fetch; prefer stale-but-attributed cache over wiping good history on a blip — never reuse across identity change.

## Links

- https://github.com/Atharva-Kanherkar/bridge-harness/pull/633
- https://github.com/Atharva-Kanherkar/bridge-harness/issues/555
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/639
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/628 (native usage menu — merged earlier same day)
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/638 (release automation — still open)
- Related: `learnings/2026-09-14-bridge-harness-target.md`
