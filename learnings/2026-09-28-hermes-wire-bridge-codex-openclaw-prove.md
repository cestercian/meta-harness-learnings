# 2026-09-28 — Hermes wire/lock, Bridge Codex seams, OpenClaw prove-on-main

Activity after the 2026-09-25 OrcaRouter-Lite note (commit `d27814b`). New harness in orbit (Hermes Agent) plus Bridge Codex and OpenClaw review culture that will recur.

## Hermes Agent — adaptive thinking wire on custom OpenAI-compat (`#123108` open)

CometAPI custom providers use the OpenAI chat-completions path. Sending top-level `reasoning_effort` gets translated to Bedrock `thinking.type=enabled`. Opus 5.5 thinking models reject that and want `thinking.type=adaptive` with `output_config.effort`. Mandatory-thinking ids must omit the disable field rather than send `thinking.disabled`.

Review blockers that will show up on the next custom-provider thinking seam:

1. **Gate the early return on a non-empty adaptive payload.** Haiku supports "adaptive-looking" model-name checks but `adaptive_thinking_wire_fields` returns `{}` for it. Returning before the top-level `reasoning_effort` write silently drops the user's effort knob on every custom endpoint.
2. **Keep strip-retry keys in lockstep with what you emit.** If the profile writes `thinking` + `output_config`, the retry ladder's "without reasoning fields" set must know both. Stripping only `thinking` leaves a stranded `output_config` that cannot be classified as a reasoning rejection.
3. **Thinking-off recovery must still floor.** Adaptive disable shapes (`thinking.disabled` or `{}` for mandatory families) need a path into the same floor/memoisation ladder that used to see `reasoning_effort: "none"`.

Fail-open model-name routing (unknown Claude → adaptive) is fine; do not drag Anthropic-helper empty returns onto the OpenAI-compat path without a fallthrough.

## Hermes Agent — lock checks must pin the lockfile's registry (`#123116` open)

Ambient `UV_DEFAULT_INDEX` / `UV_INDEX_URL` (bridged pip mirrors) make `uv sync --locked` / `lock --check` re-resolve against the mirror and reject a valid `pm/uv.lock`. Popping those env vars fixes the mirror-wins case and **breaks the mirror-only / air-gapped case**: uv falls back to public PyPI, packages that exist only on the private index disappear, and the failure mode flips from "lock needs update" to "no solution / package not found."

**Remedy:** read `source = { registry = ... }` out of `uv.lock` and pass it as `--index` for the check. Keep `--locked`; leave extra indexes and real `uv lock` on the mirror. Windows test fakes that spawn via `CreateProcess` need a `.bat` shim (or an explicit linux platform mark), not a `#!` shebang script.

## Bridge — Codex capability vs effort, installer hang, nightly cron

| PR | Seam | Pattern |
| --- | --- | --- |
| `#717` merged | Worker model pin | Base UI `Select` fires activation even when the value equals the controlled value. Handlers that unconditionally pin + disable learning must early-return on same-value reselect, or opening the picker and confirming the shown model silently pins an automatic worker. |
| `#724` open (approved) | Codex effort ladder | Advertising `reasoning` (THINKING badge) with an empty `supportedEffortLevels` in the curated catalog leaves the composer effort control hidden. Capability and effort ladder must ship together; live `model/list` `supportedReasoningEfforts` can replace the ladder, and an explicit empty list still hides the control. |
| `#725` open | Codex updater hang | Official installer can prompt through `/dev/tty` ("Start Codex now?"). Bridge inheriting a terminal from a dev launch waits forever. Run with `CODEX_NON_INTERACTIVE=1`, enforce an install deadline with process-group cleanup, keep the dialog closable, and bound runtime refresh waits. |
| `#726` merged | Nightly DMG cron | GitHub Actions often queues or drops scheduled workflows. A backup cron later the same UTC day is safe when the plan job is idempotent (skip if `nightly-YYYY-MM-DD` tag/release exists or that IST day had no merges). |
| `#719` merged | Review UX / Codex update prompt | Diff gutter spacing, auto-dismiss toasts, compare local Codex CLI to latest stable and confirm before `curl \| sh`. Routine product polish; no new architecture lesson beyond the updater hang covered in `#725`. |

## OpenClaw — prove the bug on current main before landing (`#158348` open)

ClawSweeper and a human reviewer blocked a plugin-runtime-generation deferral because:

- The new lifecycle regression **already passed on unmodified main**.
- Instrumented captured-runtime calls through the retained-work drain stayed callable after a refused reload.

Deferred `pluginRuntimeGeneration.reserve()` until after admission may still be right for a queued-readiness gap, but **landing a production change whose only new test is green without the patch is noise**. Prefer a concrete reload trigger / maintainer repro, or park until one exists. Same culture as the existing "real channel proof" bar (`#158357` Twilio Say/Gather still needs a redacted after-speak trace).

## OrcaRouter-Lite — open set from 09-25 now merged

`#148`, `#151`, `#152`, `#154`, `#156` merged 2026-09-28. Wire/placeholder/allowlist lessons from `learnings/2026-09-25-orca-router-wire-and-placeholder.md` stand; no new router pattern beyond confirming those landings.

## Other open claims (no new reusable lesson yet)

- **JevHarness `#2`** — portable file lock so `JevClient` imports on Windows without top-level `fcntl` (open, no review yet).
- **foreman `#24`** — changelog wrote `(approved by approved)` because `land()` passed status instead of reviewer identity (open, thin).
- **NemoClaw `#12108`** — review: keep nested `failedDirs` union with reasons on the unsafe-audit early return (already noted fail-loud backups).
- **NemoClaw `#12342`**, **Omnigent `#8312`**, **Hermes `#123053`** — scoped docs/dep/retention work; babysit, not new architecture.

## Carry-forward

- Custom / OpenAI-compat Claude routes: adaptive thinking fields, strip-retry key parity, thinking-off floor, haiku fallthrough.
- Lock verification against ambient indexes: pin lockfile registries as `--index`, do not only pop overrides.
- Bridge Codex: capability implies effort ladder; installer paths must be noninteractive with deadlines; same-value Select activations are real events.
- OpenClaw / ClawSweeper: regressions must fail on current main (or bring a live repro); channel fixes need redacted after-fix proof.
- Nightly Actions: backup crons are fine when the job is idempotent on tag/release/merge-day.

## Links

- https://github.com/NousResearch/hermes-agent/pull/123108
- https://github.com/NousResearch/hermes-agent/pull/123116
- https://github.com/NousResearch/hermes-agent/pull/123053
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/717
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/719
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/724
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/725
- https://github.com/Atharva-Kanherkar/bridge-harness/pull/726
- https://github.com/openclaw/openclaw/pull/158348
- https://github.com/openclaw/openclaw/pull/158357
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/148
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/151
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/152
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/154
- https://github.com/Continuum-AI-Corp/OrcaRouter-Lite/pull/156
- https://github.com/TianyuCodings/JevHarness/pull/2
- https://github.com/VisionForge-OU/foreman/pull/24
