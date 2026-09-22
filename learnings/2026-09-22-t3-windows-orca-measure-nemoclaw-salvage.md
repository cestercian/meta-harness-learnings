# 2026-09-22 — T3 Windows spawn/UI honesty, measure-before-document SDKs, NemoClaw salvage

Activity after the 2026-09-21 note (commit `6173dfd`). Patterns worth carrying.

## T3 Code — Windows spawn honesty + UI/diff fail-soft (open wave)

Five new upstream fixes on `pingdotgg/t3code` (Sep 21–22). Shared theme: match what Node/Windows actually do, and never let an identical control mean two different things.

| PR | Seam | Pattern |
| --- | --- | --- |
| `#12863` open | Windows `.cmd`/`.bat` + Node 24 | `resolveSpawnCommand` must not return a shim path **plus** an args array with `shell: true` (DEP0190). For `.cmd`/`.bat`, return a **single command string** and empty `args` so Node expands `%ComSpec% /d /s /c "…"`. Keep `.exe`/`.com` on `shell: false`. |
| `#12878` open | Antigravity custom `binaryPath` | Rigid same-directory `localharness_external` sibling checks fail on `.cmd`/`.bat` wrappers and hang ACP through `cmd.exe /c`. **Unwrap** the wrapper to the real `agy_acp_server.exe`, pair the matching `localharness_external.exe` (including managed releases), and spawn the `.exe` directly so ACP stdio stays intact. Reject ambiguous wrappers instead of resolving to the wrong binary. |
| `#12889` open | Codex app-server JSONL stdin | Queue-depth caps (`MAX_BUFFERED_RAW_MESSAGES`) do not bound fragment accumulation. Add a **per-message byte ceiling** (128 MiB) checked as fragments arrive and before `remainder.join()` / parse; overflow fail-closes the session (`CodexAppServerTransportError` / `read-input-stream`). |
| `#12890` open | Provider update icons | List and editor `ArrowUpCircle` glyphs looked identical but did different things (copy command vs open Update popover). Identical affordances must share one control (`ProviderUpdateAvailableControl`); keep copy as a **distinct** action inside the popover so read-only sessions can still copy without getting Update now. |
| `#12906` open | PR / Diff file tree | Pierre’s nested tree cannot represent both `office` and `office/config.ts` when git replaces a file/symlink with a directory (or the reverse). Detect prefix collision and fall back to a **flat file list** that keeps every path and selection target; ordinary diffs keep the nested tree. |

Issues: #12797, #12752, #12884, #12886, #12887.

## OrcaReplay — measure SDK env behaviour before documenting (`#99` merged)

Docs claimed `@ai-sdk/openai` takes its origin only as a constructor arg and reads nothing from the environment. Measured on 3.0.112 that has been false since 2.0.41: `createOpenAI({ apiKey })` and the bare `openai` provider honour `OPENAI_BASE_URL`; an explicit `baseURL` still wins. Docs now recommend `orca record generic-openai --` for that case and keep the `node` fetch preload when the origin is compiled into source.

**Posture:** when integration docs assert “SDK does not read env X”, verify against a current package version before shipping the claim.

## NemoClaw — adapter-runtime probes + salvage when Verified/stale-base blocks landing

`#12104` (credential probe in generated `mcp add` policy) was closed as superseded by maintainer-salvaged `#12175` (merged; closes #12065).

### Design pivot (review-driven)

First cut put `/usr/bin/curl` / `/usr/local/bin/curl` on the persistent generated `mcp-bridge-<server>` binary allowlist so OpenShell’s CONNECT attribution (`/proc/<pid>/exe`) would allow the health probe. Review pushed the other way: **do not widen the policy with interactive curl**. Probe through the selected adapter runtime instead (`buildMcpAdapterHttpProbeCommand` — Node `fetch` for OpenClaw, `urllib` for Hermes/Deep Agents) so CONNECT is attributed to an already-allowed socket owner. Generated binaries assert curl is absent.

### Contrib posture

NVIDIA runner / Verified + stale-base gates can block an otherwise-ready fork PR while a maintainer re-lands the same fix on main. When that happens: close the fork PR as superseded (link the salvage), thank the reviewer, and update notes to the **landed** seam — not the closed PR number alone. Still respect the **5 open PR cap**.

Related still-open seams unchanged in spirit: backup fail-loud `#12108`, devices-approve exit `#12109`, local tool disclosure `#12110`, policy-remove strip `#12111`.

## NeMo Agent Toolkit — think-wrapped ReAct landed

`#2227` merged (was open in the 2026-09-18 note): recover inner `<think>` text only when it looks like ReAct (`Action:` / `Final Answer:`); skip empty scratchpad replay. Empty-stream retry re-raise `#2226` remains open.

## Bridge — unknown `$harness` shortcuts are commands, not chat (`#678` merged)

Command-shaped unknown `$harness` tokens must not fall through as ordinary chat text. Preserve the draft, show available harness IDs with a close-match hint, and keep currency-like / mid-sentence `$` on the normal chat path.

## Carry-forward

- Windows provider spawn: DEP0190-safe `.cmd`/`.bat` packing; unwrap launcher wrappers to the real ACP `.exe` pair.
- Transport buffers: queue depth ≠ byte ceiling — cap fragments before join/parse.
- UI: identical glyphs → identical actions; keep secondary actions visually distinct.
- Diff trees: file↔dir path collisions need a flat fallback the nested model cannot represent.
- Capture/docs: measure current SDK/env behaviour before asserting “does not read X”.
- Sandbox policy: prefer probing via already-allowed adapter runtimes over expanding binary allowlists with curl.
- NVIDIA contrib: expect salvage merges under Verified/stale-base; close superseded forks cleanly; keep ≤5 open PRs.
- Control-plane composers: reject unknown harness shortcuts; do not silently chat them.

## Links

- https://github.com/pingdotgg/t3code/pull/12863
- https://github.com/pingdotgg/t3code/pull/12878
- https://github.com/pingdotgg/t3code/pull/12889
- https://github.com/pingdotgg/t3code/pull/12890
- https://github.com/pingdotgg/t3code/pull/12906
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/99
- https://github.com/NVIDIA/NemoClaw/pull/12104
- https://github.com/NVIDIA/NemoClaw/pull/12175
- https://github.com/NVIDIA/NeMo-Agent-Toolkit/pull/2227
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/678
