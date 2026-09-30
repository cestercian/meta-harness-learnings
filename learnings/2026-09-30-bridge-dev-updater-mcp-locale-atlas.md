# 2026-09-30 — Bridge dev-binary updater, MCP locale caches, ATLAS absolute suite paths

Activity after the 2026-09-29 Bridge beta/updater note (commit `1216881`). Real new signals: Bridge `#745` (open, CI green), mcp-memory-service `#1382` (open; greptile P1 fixed), ATLAS `#276` (merged), Hermes `#123053` (merged; already noted on `harnesses/hermes-agent.md`).

## Bridge — refuse updater installs from unbundled Cargo builds (`#745`)

`#738` added the opt-in beta nightly feed. A real install from a development binary then wrecked the local tree: Tauri's macOS updater treats the executable's parent as the install target when it is **outside** an `.app` bundle. Running from `src-tauri/target/debug/bridge-deck`, the nightly install replaced `target/debug` with app contents; the Cargo binary disappeared while `/Applications/Bridge.app` stayed on the older stable.

**Remedy (still open on `#745`):** hide Install in the Vite/dev UI; guard both update channels in the native command so unbundled development binaries cannot run replacement. Packaged `.app` installs are unchanged.

## Bridge — toast channel/version must own the install handle (`#745` follow-through)

The 09-29 note already flagged clearing `cachedUpdate` / `cachedNightlyVersion` at check start. `#745` implements the fix: install the **channel and version shown in the toast**, rechecking that release before replacement; a later automatic check must not wipe a mutable global cache and leave the toast stuck on "Installing…". Replace a stale beta-feed error when a later check finds an update; ignore results from superseded manual checks.

## mcp-memory-service — one locale switch, no import-time freeze (`#1382`)

`MCP_LOCALE` is meant to drive every locale-aware subsystem, but harvest / rewriter / Kiro still read `HARVEST_LOCALE` (or built formatters at import). Setting only `MCP_LOCALE` silently stayed English.

| Pattern | Detail |
| --- | --- |
| Precedence | Shared `get_active_locales()` so `MCP_LOCALE` wins; `HARVEST_LOCALE` remains fallback |
| Late env | Do not `@lru_cache` the **resolved** locale across the process — NLI at import freezes the first read. Re-read env each call (cache parsing only) |
| Formatters | Hold factories in `_FORMATTERS`, build Kiro when requested, so a locale set after import is honoured |

Greptile P1 on the first push caught the stale Kiro path; fixed in a follow-up commit with a test that does **not** clear the cache first.

## ATLAS — resolve suite roots before Docker mounts (`#276` merged)

`driver.py check` / `run` passed relative suite paths (e.g. `.`) into grader `docker run -v <path>:…`. Docker treats a relative host path as a **named volume** and fails. Resolve the suite root to an absolute path once at `load_suite` entry so every later mount is absolute. Relative and absolute paths to the same suite must produce the same grader mount.

## Status only

- Hermes `#123053` merged (desktop `lucide-react` pin) — already on `harnesses/hermes-agent.md`.
- Bridge `#747` open (opt-in auto-expand edit activity) — product UX, no architecture lesson.
- hivegate `#76` open (docs: `DEFAULT_CHAT_MODEL` in `.env.example`); liberzon approved.
- OpenClaw / NemoClaw / Omnigent / OrcaRouter-Lite `#145` / t3 / Prime: quiet or already covered; Prime still vouch-gated.

## Carry-forward

- Tauri (and similar) updaters: never install into unbundled `target/debug`; gate Install on a real `.app` / package root.
- Dual-channel updaters: toast state owns the install handle; rechecks replace or clear deliberately, never wipe at check start.
- Locale / config env: one public switch; subsystem aliases are fallbacks; never freeze resolution at import behind an unbounded cache.
- Eval harnesses that shell out to Docker: absolutize host paths at the driver boundary.
- Prior 09-28 / 09-29 Bridge nightly + Hermes / OpenClaw carry-forwards still stand.

## Links

- https://github.com/Atharva-Kanherkar/bridge-harness/pull/745
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/747
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/738
- https://github.com/doobidoo/mcp-memory-service/pull/1382
- https://github.com/inferstep/ATLAS/pull/276
- https://github.com/NousResearch/hermes-agent/pull/123053
- https://github.com/hivegate-ai/hivegate/pull/76
