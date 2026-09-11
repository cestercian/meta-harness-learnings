# Orca ecosystem (Continuum)

Distinguish:

1. **onorca.dev** — commercial Orca IDE / agent launcher (Prime Agent appears as a supported agent). Not the same as Continuum’s open repos.
2. **OrcaReplay** — https://github.com/Continuum-AI-Corp/OrcaReplay — record/replay/fork agent runs. Capture is growing past proxy I/O (origin of each model call; agent structure the proxy cannot see). Control flags that cannot be honoured should fail loud, not silently no-op.
3. **OrcaCode Review** — https://github.com/Continuum-AI-Corp/Orca-Code-Review — PR review harness + merge gate via OrcaRouter. Judge / gate failures fail closed and name the gate.
4. **OrcaRouter-Lite** — https://github.com/Continuum-AI-Corp/OrcaRouter-Lite — self-hosted OpenAI-compatible router.

Contribution tip: prefer clear local repros that do not need paid router keys when possible; treat auth/IDOR issues carefully (responsible disclosure if needed).