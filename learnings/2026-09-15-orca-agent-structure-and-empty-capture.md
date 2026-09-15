# 2026-09-15 — OrcaReplay: agent-structure layer landed; empty capture and viewer must follow

Upstream Continuum since the 2026-09-11 / 2026-09-14 notes. Not our PRs — posture to carry when contributing.

## Sixth capture layer landed (`#65`)

`feat: record the agent structure a proxy cannot see` merged. First layer that records what orca was *told* (SDK tracing: which agent owned a turn, that a handoff happened and from whom, input guardrails) rather than only HTTP I/O the proxy observed. Proxy-alone transcripts stay incomplete for multi-agent / guardrail runs.

## Empty capture must say so (`#83`, open)

Agent-spans bootstrap used `except ImportError: return` when `orcareplay-openai-agents` was missing. Measured: same handoff agent with vs without the package — without, the layer comes back empty and looks like a successful quiet capture. Same fail-loud family as TLS `--model`, flag-value fallbacks, and scrub under-count: “captured nothing” is not success when the layer could not run.

## Viewer must grow with the layer (`#84`, open)

`#65` put structure events into the trace; `buildTimeline` had no row types for the three events a proxy cannot produce (handoff / agent ownership / guardrail). The layer exists and is invisible — capture without display is half a harness.

## Carry-forward

- Prefer patches that keep capture, fail-loud, and viewer in lockstep when a new layer ships.
- Treat silent empty / missing optional package as a loud “layer unavailable,” not a green zero.

## Links

- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/65
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/83
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/84
- Related: `learnings/2026-09-11-orca-fail-loud-and-capture.md`, `learnings/2026-09-14-orca-vision-capture-and-scrub.md`
