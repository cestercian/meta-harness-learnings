# 2026-09-11 — Orca: fail loud + capture what the proxy cannot see

Maintainer posture on Continuum Orca* aligns with (and generalizes) our TLS `--model` fix (`#67`).

## Fail loud when a control cannot be honoured
- OrcaReplay `#50`: flag *values* that cannot be applied no longer silently fall back (`--model` without a value, duplicate `--match`, etc.). Same class as silent path-skew success — report failure instead of a green wrong run.
- OrcaCode Review `#52`: when the L2 judge is unavailable, **fail closed** and name which gate closed (no silent “review skipped / discarded after 11 minutes”). Retries belong in the shared fact proxy, not nested in `judge.mjs` (multiply 502 storms).

## Capture layers beyond the HTTP proxy
- OrcaReplay `#58`: record **which origin answered** each model call (not just orca’s local proxy address). A leftover gateway in `~/.orca/config.json` was invisible in traces.
- OrcaReplay `#65` (open): sixth capture layer for **agent structure the proxy cannot see** (handoffs / input guardrails measured first, then recorded). Direction: proxy I/O alone is not a full harness transcript.

## Carry-forward
Prefer patches that make unsupported or partial paths loud, and prefer telemetry that answers “who answered / what structure ran” without needing paid router keys for the repro.

## Links
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/50
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/58
- https://github.com/Continuum-AI-Corp/OrcaReplay/pull/65
- https://github.com/Continuum-AI-Corp/Orca-Code-Review/pull/52
- Related prior note: `learnings/2026-09-10-orcareplay-tls-model.md`