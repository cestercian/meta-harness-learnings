# 2026-09-23 — Bridge usage/replay seams, NemoClaw salvage, OpenCode bin-name close

Activity after the 2026-09-22 note (commit `1d05278`). Patterns worth carrying.

## Bridge — session usage, warm-worker cache, transcript recovery (`#682` merged)

Four seams landed (Fixes #537, #144; addresses #531 G8/G11 and #292's anonymous-row). Shared theme: recovery and attribution must stay honest across reroute, warm reuse, and corrupt history.

### Usage attribution

Price and attribute rerouted usage to the **serving** model. Session and prompt models stay what Bridge asked for.

Owner review: flipping `usage_ledger.model` without backfilling older rows mixes two attribution semantics across the upgrade. Audit other ledger consumers (`learning_router`, workspaces) that may have wanted the requested model.

### Context pressure / warm workers

Send only the variable prompt suffix to compatible hot workers. Reset stale context gauges on model, harness, or provider-binding changes via a durable SQL watermark (`context_usage_after_id`), not wall-clock.

Owner: verify the prefix was actually delivered and is still resident (native resume, provider compaction, partial `send_turn`). Otherwise fall back to full instructions. One happy-path test is not enough.

### Anonymous tool activity

Keep empty anonymous tool starts out of visible activity until identity, output, or a terminal result. Adopt later progress titles. Keep recognized subagent cards.

Caveat: filtering `pendingIdentity` from `visibleItems` can also clear the streaming indicator. Filter at render time, or compute streaming from the pre-filter items. Abandoned anonymous calls must still show that something was attempted.

### Degraded replay

Unreadable stored entries become `entry.invalid` carriers at the original sequence (`status: degraded`) so pagination, reconnect, and lag recovery continue. The malformed payload stays off the wire.

Owner: map `reason` to closed codes (not raw `error.to_string()`), dedupe diagnostics per entry, and clarify the forest-snapshot path versus the replay path. The UI may still hard-fail on forest.

### CI hygiene

Immutable committed `fake-gh.sh` fixtures avoid Linux races that spawn a newly written executable.

### Packaging

An optional AppImage `linuxdeploy` failure on main is not a delivery gate for these fixes. Standard frontend, Rust, sidecar, and macOS smoke are.

`#683` (supervised composer dictation / sherpa-onnx) stays a draft until live mic smoke — not a landed pattern yet.

## NemoClaw — second salvage wave (`#12109` closed)

Maintainer merged `#12178` (Rebecca Sliter), adopting the devices-approve exit fix onto a same-repository branch so NVIDIA SDK-backed CI can run. The landed commit credits the original patch author and Rebecca Sliter. Closes #12064.

Same posture as `#12104` → `#12175`: Verified and runner gates block fork PRs. Expect salvage, close superseded forks cleanly, and keep ≤5 open PRs.

Landed seam: drain stdout and stderr, then `exit(0)` after Approved on the local fallback, so leftover gateway handles do not hang `nemoclaw connect`.

## OpenCode — product naming obsolete (`#49807` closed)

Closed after @simonklee noted the binary is just `opencode` now. The dual-name / `opencode2` resume-hint path is no longer needed.

argv0 and bin-name UX assumptions rot when a product collapses aliases. Confirm current install names before shipping resume hints.

## T3 — deepen `#12889` (still open)

Macroscope: fragment length must count **UTF-8 bytes** (`TextEncoder`), not UTF-16 code units after `decodeText`. Multibyte input can otherwise exceed the 128 MiB ceiling by about 3×. Inline transport errors at the failure boundaries; a helper that only wraps `CodexAppServerTransportError` is the wrong shape. Product-default runtime behavior still needs human review (Macroscope “Not approved” for the default 128 MiB kill).

## Carry-forward

- Bridge usage ledger: serving vs requested model must be explicit across migrations and backfills.
- Warm-worker suffix-only delivery needs negative tests and residency guarantees, not only a compatibility-key happy path.
- Replay degradation: closed reason codes, and forest-path parity with replay carriers.
- NVIDIA: expect salvage merges onto same-repo branches under runner and Verified gates.
- OpenCode: re-check binary and product names before argv0 UX.
- Transport ceilings: measure UTF-8 bytes after decode, not JS string length.

## Links

- https://github.com/Atharva-Kanherkar/bridge-harness/pull/682
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/683
- https://github.com/NVIDIA/NemoClaw/pull/12109
- https://github.com/NVIDIA/NemoClaw/pull/12178
- https://github.com/anomalyco/opencode/pull/49807
- https://github.com/pingdotgg/t3code/pull/12889
