# 2026-09-16 — OrcaReplay: empty-capture / viewer landed; MCP check found a bug; gateway fork honesty

Upstream Continuum since the 2026-09-15 agent-structure note. Not our PRs — posture to carry.

## Empty capture + viewer closed the lockstep (`#83`, `#84` merged)

What yesterday’s note called open is now on `main`: missing `orcareplay-openai-agents` announces empty capture instead of a quiet zero (`#83`); timeline rows exist for the three proxy-invisible structure events (`#84`). Capture / fail-loud / viewer stayed in the same release train as `#65`.

## Thickest layer had no integration check — until the check found a bug (`#87`)

MCP is ~1900 lines (`mcp-shim` + `cli/src/mcp.ts`) and was the only capture layer with **no** end-to-end “recorded session comes back” assertion. `replay.done` counters cannot speak for MCP either. Adding the missing check surfaced a real defect — same pattern as Windows-red-baseline hiding `#69`: absence of a harness check is itself a risk surface.

## Gateway path honesty (`#85`, `#88`, `#89`)

Live `orca push` / `pull` against OrcaRouter on Windows found two independent blockers that stop a run reaching the gateway (`#85`). `push` now actually takes the `--fs` the help offered (`#88`). A run pulled from a gateway can be forked locally — and the CLI says so when it cannot (`#89`), matching the console’s “fork it locally” promise instead of failing opaque.

## Carry-forward

- Prefer shipping the integration check with (or before) a thick capture layer — the check is how you discover the bug, not just how you prevent regressions.
- Gateway / CLI UX that promises fork or push must fail loud with the real reason when the path is impossible.
- Treat optional packages and multimodal seams the same way: announce empty / unavailable, do not look green.

## Links

- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/83
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/84
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/85
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/87
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/88
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/89
- Related: `learnings/2026-09-15-orca-agent-structure-and-empty-capture.md`
