# 2026-09-10 — OrcaReplay: silent `--model` on TLS-intercept forks

## Bug
`orca replay --from N --model X` on a TLS-intercepted capture (e.g. Hermes) echoed `model=X` but called the recorded model. `replayed=0 live=0` meant substitution never ran (`goLive` skipped; intercept live path forwarded client bytes).

## Fix shipped
Upstream PR: https://github.com/Continuum-AI-Corp/OrcaReplay/pull/67 (Fixes #49)

- Intercept live path can return `InterceptForward` (rewritten body → same CONNECT origin).
- Same-provider `--model` rewrites JSON + counts `liveCalls`.
- Cross-provider `--model` → 400 (cannot leave intercepted host) instead of silent no-op.

## Harness learning
Control-plane flags that only work on one capture path (base-URL proxy vs TLS MITM) need either path parity or loud failure. Silent success is worse than unsupported.
