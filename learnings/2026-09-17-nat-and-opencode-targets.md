# 2026-09-17 — Add NeMo Agent Toolkit + OpenCode to the harness map

User asked to look at NVIDIA’s harness and OpenCode as contribution targets (in addition to Prime / T3 / Orca / OpenClaw / Bridge). Prioritize:

1. **NVIDIA/NeMo-Agent-Toolkit** — first NVIDIA surface to hunt
2. **anomalyco/opencode** — coding agent used under T3/Bridge

OpenShell / NemoClaw stay documented as related NVIDIA runtime surfaces but secondary unless asked.

First NAT claim in flight: [#2223](https://github.com/NVIDIA/NeMo-Agent-Toolkit/issues/2223) (`patch_with_retry` drops streaming exceptions on retry with one-shot async iterators).
